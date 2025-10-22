# Chapitre 16 : Étude de cas complète

> **Objectif:** Présenter un exemple concret de projet de Brevet fédéral de bout en bout, servant de modèle et d'inspiration.

---

## 📋 Étude de cas: Plateforme de gestion d'événements "EventHub"

### Contexte

**Problématique:**
Les organisateurs d'événements (conférences, meetups, formations) utilisent souvent des outils fragmentés: un pour les inscriptions, un autre pour les paiements, un troisième pour le check-in. Cette dispersion complique la gestion et nuit à l'expérience utilisateur.

**Solution:**
EventHub est une plateforme intégrée permettant de gérer l'ensemble du cycle de vie d'un événement, de la création à l'analyse post-événement.

---

## 16.1 Phase d'analyse

### 16.1.1 Acteurs identifiés

```
👤 Organisateur
   - Créer et gérer événements
   - Configurer billets et tarifs
   - Suivre inscriptions
   - Analyser statistiques

👤 Participant
   - Découvrir événements
   - S'inscrire et payer
   - Recevoir billet QR
   - Donner feedback

👤 Staff (check-in)
   - Scanner QR codes
   - Valider entrées
   - Gérer liste participants
```

### 16.1.2 Cas d'utilisation principaux

```
UC-001: Créer un événement
Acteur: Organisateur
Préconditions: Utilisateur authentifié
Scénario nominal:
1. L'organisateur accède au tableau de bord
2. Clique sur "Nouvel événement"
3. Remplit les informations (titre, date, lieu, description)
4. Configure les types de billets et prix
5. Publie l'événement
Postconditions: Événement visible publiquement

UC-002: S'inscrire à un événement
Acteur: Participant
...

UC-003: Scanner QR code au check-in
Acteur: Staff
...
```

### 16.1.3 Exigences

**Fonctionnelles:**
- RF-001: Gestion CRUD événements
- RF-002: Système de billetterie avec paiement Stripe
- RF-003: Génération QR codes uniques
- RF-004: Application mobile check-in
- RF-005: Tableau de bord statistiques temps réel
- RF-006: Notifications email automatiques
- RF-007: Export données (CSV, PDF)

**Non fonctionnelles:**
- RNF-001: Temps de réponse API < 500ms (95th percentile)
- RNF-002: Support 10,000 participants par événement
- RNF-003: Disponibilité 99.5%
- RNF-004: Données sensibles chiffrées (RGPD)
- RNF-005: Interface responsive (mobile/tablet/desktop)

---

## 16.2 Architecture technique

### 16.2.1 Vue d'ensemble

```
┌─────────────────────────────────────────────────────────┐
│                    Load Balancer (NGINX)                │
└────────────┬────────────────────────────────────────────┘
             │
    ┌────────┼─────────┐
    │        │         │
┌───▼───┐ ┌──▼───┐ ┌──▼───┐
│ Web   │ │ API  │ │ API  │
│ App   │ │ (1)  │ │ (2)  │
│(React)│ └──┬───┘ └──┬───┘
└───────┘    │        │
             │        │
        ┌────▼────────▼────┐      ┌──────────────┐
        │   PostgreSQL     │      │    Redis     │
        │   (Primary)      │      │   (Cache)    │
        └──────────────────┘      └──────────────┘
             │
        ┌────▼────────┐
        │ PostgreSQL  │
        │  (Replica)  │
        └─────────────┘

Services externes:
- Stripe (paiements)
- SendGrid (emails)
- AWS S3 (images événements)
```

### 16.2.2 Stack technologique

**Backend:**
- **Framework:** Node.js + Express
- **Base de données:** PostgreSQL 14
- **ORM:** Prisma
- **Cache:** Redis
- **Auth:** JWT + bcrypt
- **Tests:** Jest + Supertest

**Frontend:**
- **Framework:** React 18 + TypeScript
- **State management:** Redux Toolkit
- **UI:** Material-UI
- **Forms:** React Hook Form + Yup
- **Tests:** Jest + React Testing Library

**Mobile:**
- **Framework:** React Native + TypeScript
- **Navigation:** React Navigation
- **QR Code:** react-native-camera
- **State:** Redux Toolkit

**DevOps:**
- **Conteneurs:** Docker + Docker Compose
- **CI/CD:** GitHub Actions
- **Hébergement:** DigitalOcean Droplet
- **Monitoring:** Prometheus + Grafana
- **Logs:** Winston + Elasticsearch

### 16.2.3 Modèle de données

```sql
-- Users
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    role VARCHAR(20) DEFAULT 'participant',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Events
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    organizer_id INTEGER REFERENCES users(id),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    location VARCHAR(255),
    start_date TIMESTAMP NOT NULL,
    end_date TIMESTAMP NOT NULL,
    capacity INTEGER,
    image_url VARCHAR(500),
    status VARCHAR(20) DEFAULT 'draft',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Ticket Types
CREATE TABLE ticket_types (
    id SERIAL PRIMARY KEY,
    event_id INTEGER REFERENCES events(id) ON DELETE CASCADE,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    quantity_available INTEGER NOT NULL,
    quantity_sold INTEGER DEFAULT 0
);

-- Registrations
CREATE TABLE registrations (
    id SERIAL PRIMARY KEY,
    event_id INTEGER REFERENCES events(id),
    user_id INTEGER REFERENCES users(id),
    ticket_type_id INTEGER REFERENCES ticket_types(id),
    qr_code VARCHAR(255) UNIQUE NOT NULL,
    payment_status VARCHAR(20) DEFAULT 'pending',
    payment_intent_id VARCHAR(255),
    checked_in_at TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(event_id, user_id, ticket_type_id)
);

CREATE INDEX idx_registrations_event ON registrations(event_id);
CREATE INDEX idx_registrations_qr ON registrations(qr_code);
```

---

## 16.3 Implémentation technique

### 16.3.1 Backend - Exemple API

```javascript
// src/controllers/eventController.js
const eventService = require('../services/eventService');

exports.createEvent = async (req, res, next) => {
    try {
        const event = await eventService.createEvent({
            ...req.body,
            organizerId: req.user.id
        });

        res.status(201).json({
            success: true,
            data: event
        });
    } catch (error) {
        next(error);
    }
};

exports.getEvents = async (req, res, next) => {
    try {
        const { page = 1, limit = 20, search, status } = req.query;
        
        const result = await eventService.getEvents({
            page: parseInt(page),
            limit: parseInt(limit),
            search,
            status
        });

        res.json({
            success: true,
            data: result.events,
            pagination: {
                currentPage: result.page,
                totalPages: result.totalPages,
                totalItems: result.totalCount
            }
        });
    } catch (error) {
        next(error);
    }
};

// src/services/eventService.js
const prisma = require('../config/database');
const { generateQRCode } = require('../utils/qrCode');
const cache = require('../config/redis');

exports.createEvent = async (eventData) => {
    // Validation
    if (new Date(eventData.startDate) < new Date()) {
        throw new Error('Start date must be in the future');
    }

    // Créer l'événement avec types de billets
    const event = await prisma.event.create({
        data: {
            ...eventData,
            ticketTypes: {
                create: eventData.ticketTypes
            }
        },
        include: {
            ticketTypes: true,
            organizer: {
                select: { id: true, firstName: true, lastName: true, email: true }
            }
        }
    });

    // Invalider cache
    await cache.del('events:public');

    return event;
};

exports.registerForEvent = async (userId, eventId, ticketTypeId) => {
    return await prisma.$transaction(async (tx) => {
        // Vérifier disponibilité
        const ticketType = await tx.ticketType.findUnique({
            where: { id: ticketTypeId }
        });

        if (ticketType.quantitySold >= ticketType.quantityAvailable) {
            throw new Error('Tickets sold out');
        }

        // Générer QR code unique
        const qrCode = generateQRCode();

        // Créer inscription
        const registration = await tx.registration.create({
            data: {
                userId,
                eventId,
                ticketTypeId,
                qrCode,
                paymentStatus: 'pending'
            }
        });

        // Incrémenter compteur
        await tx.ticketType.update({
            where: { id: ticketTypeId },
            data: { quantitySold: { increment: 1 } }
        });

        return registration;
    });
};

// src/routes/eventRoutes.js
const express = require('express');
const router = express.Router();
const eventController = require('../controllers/eventController');
const { authenticate, authorize } = require('../middleware/auth');
const { validate } = require('../middleware/validation');
const { createEventSchema } = require('../schemas/eventSchemas');

router.get('/', eventController.getEvents);
router.get('/:id', eventController.getEventById);

router.post('/',
    authenticate,
    authorize('organizer'),
    validate(createEventSchema),
    eventController.createEvent
);

router.patch('/:id',
    authenticate,
    authorize('organizer'),
    eventController.updateEvent
);

module.exports = router;
```

### 16.3.2 Frontend - Exemple composant

```typescript
// src/components/EventCard.tsx
import React from 'react';
import { Card, CardContent, CardActions, Typography, Button, Chip } from '@mui/material';
import { Event as CalendarIcon, Location, People } from '@mui/icons-material';
import { format } from 'date-fns';

interface EventCardProps {
    event: {
        id: number;
        title: string;
        description: string;
        startDate: string;
        location: string;
        capacity: number;
        registrationsCount: number;
        imageUrl?: string;
    };
    onRegister: (eventId: number) => void;
}

const EventCard: React.FC<EventCardProps> = ({ event, onRegister }) => {
    const spotsLeft = event.capacity - event.registrationsCount;
    const isFull = spotsLeft === 0;

    return (
        <Card sx={{ height: '100%', display: 'flex', flexDirection: 'column' }}>
            {event.imageUrl && (
                <img
                    src={event.imageUrl}
                    alt={event.title}
                    style={{ height: 200, objectFit: 'cover' }}
                />
            )}
            <CardContent sx={{ flexGrow: 1 }}>
                <Typography gutterBottom variant="h5" component="h2">
                    {event.title}
                </Typography>
                
                <Typography
                    variant="body2"
                    color="text.secondary"
                    sx={{ mb: 2 }}
                    noWrap
                >
                    {event.description}
                </Typography>

                <Box sx={{ display: 'flex', alignItems: 'center', gap: 1, mb: 1 }}>
                    <CalendarIcon fontSize="small" color="action" />
                    <Typography variant="body2">
                        {format(new Date(event.startDate), 'PPP')}
                    </Typography>
                </Box>

                <Box sx={{ display: 'flex', alignItems: 'center', gap: 1, mb: 1 }}>
                    <Location fontSize="small" color="action" />
                    <Typography variant="body2">{event.location}</Typography>
                </Box>

                <Box sx={{ display: 'flex', alignItems: 'center', gap: 1 }}>
                    <People fontSize="small" color="action" />
                    <Typography variant="body2">
                        {spotsLeft > 0 ? `${spotsLeft} places restantes` : 'Complet'}
                    </Typography>
                </Box>
            </CardContent>

            <CardActions>
                <Button
                    size="small"
                    variant="contained"
                    fullWidth
                    disabled={isFull}
                    onClick={() => onRegister(event.id)}
                >
                    {isFull ? 'Complet' : "S'inscrire"}
                </Button>
            </CardActions>
        </Card>
    );
};

export default EventCard;
```

### 16.3.3 Tests

```javascript
// tests/integration/events.test.js
const request = require('supertest');
const app = require('../../src/app');
const { generateAuthToken } = require('../../src/utils/auth');

describe('Events API', () => {
    let authToken;
    let organizerId;

    beforeAll(async () => {
        // Setup test user
        const user = await createTestUser({ role: 'organizer' });
        organizerId = user.id;
        authToken = generateAuthToken(user);
    });

    describe('POST /api/events', () => {
        it('should create a new event', async () => {
            const eventData = {
                title: 'Tech Conference 2024',
                description: 'Annual tech conference',
                location: 'Zurich Convention Center',
                startDate: '2024-12-15T09:00:00Z',
                endDate: '2024-12-15T18:00:00Z',
                capacity: 500,
                ticketTypes: [
                    { name: 'Standard', price: 50, quantityAvailable: 400 },
                    { name: 'VIP', price: 150, quantityAvailable: 100 }
                ]
            };

            const response = await request(app)
                .post('/api/events')
                .set('Authorization', `Bearer ${authToken}`)
                .send(eventData)
                .expect(201);

            expect(response.body.success).toBe(true);
            expect(response.body.data.title).toBe(eventData.title);
            expect(response.body.data.ticketTypes).toHaveLength(2);
        });

        it('should return 400 for invalid date', async () => {
            const eventData = {
                title: 'Past Event',
                startDate: '2020-01-01T09:00:00Z',
                // ... autres champs
            };

            await request(app)
                .post('/api/events')
                .set('Authorization', `Bearer ${authToken}`)
                .send(eventData)
                .expect(400);
        });
    });

    describe('POST /api/events/:id/register', () => {
        it('should register user for event', async () => {
            const event = await createTestEvent();
            const participantToken = generateAuthToken(await createTestUser());

            const response = await request(app)
                .post(`/api/events/${event.id}/register`)
                .set('Authorization', `Bearer ${participantToken}`)
                .send({ ticketTypeId: event.ticketTypes[0].id })
                .expect(201);

            expect(response.body.data.qrCode).toBeDefined();
            expect(response.body.data.paymentStatus).toBe('pending');
        });

        it('should prevent double registration', async () => {
            // ... test
        });

        it('should return 409 when event is full', async () => {
            // ... test
        });
    });
});
```

---

## 16.4 Résultats et métriques

### 16.4.1 Objectifs atteints

```
✅ Fonctionnalités implémentées: 100% (28/28 user stories)
✅ Couverture tests backend: 87%
✅ Couverture tests frontend: 82%
✅ Performance API: 95th percentile = 380ms ✓
✅ Application mobile fonctionnelle iOS + Android
✅ CI/CD pipeline opérationnel
✅ Documentation complète (55 pages)
```

### 16.4.2 Défis surmontés

**1. Gestion des transactions avec Stripe**
- Problème: Webhooks Stripe pas reçus en environnement de développement local
- Solution: Utilisation de Stripe CLI pour forwarding webhooks + tests avec ngrok

**2. Race conditions lors inscriptions simultanées**
- Problème: Overselling de billets quand plusieurs utilisateurs s'inscrivent simultanément
- Solution: Utilisation de transactions PostgreSQL + lock pessimiste

**3. Performance du dashboard organisateur**
- Problème: Requêtes SQL lentes avec agrégations complexes
- Solution: Vues matérialisées + cache Redis + pagination côté serveur

### 16.4.3 Évolutions futures

```
Phase 2 (non réalisée dans le cadre du Brevet):
- Intégration calendrier (Google Calendar, Outlook)
- Chat en temps réel entre participants
- Recommandations événements basées sur ML
- Support multi-langues (i18n)
- API publique pour intégrations tierces
```

---

## 16.5 Évaluation critique

### 16.5.1 Points forts

✅ **Architecture modulaire:** Facilite maintenance et évolutions
✅ **Tests solides:** Confiance dans le code, détection précoce bugs
✅ **Documentation:** Code commenté, API documentée, ADRs
✅ **Sécurité:** Auth JWT, HTTPS, validation entrées, OWASP Top 10
✅ **UX:** Interface intuitive, feedback utilisateur positif

### 16.5.2 Points d'amélioration

❌ **Scalabilité limitée:** Monolithe backend, difficile à scaler horizontalement au-delà d'un certain point
→ Évolution: Architecture microservices

❌ **Monitoring basique:** Métriques limitées
→ Évolution: APM (Application Performance Monitoring) complet avec DataDog

❌ **Offline support mobile:** App mobile nécessite connexion
→ Évolution: Synchronisation offline avec queue

---

## 📝 Conclusion

Ce projet a permis de mettre en pratique l'ensemble des compétences requises pour le Brevet fédéral: de l'analyse des besoins à la mise en production, en passant par la conception d'architecture, le développement full-stack, les tests, le DevOps et la documentation.

Les défis rencontrés ont renforcé mes compétences en résolution de problèmes et m'ont confronté à des situations réelles rencontrées en entreprise. Le projet est fonctionnel, déployé et utilisable, démontrant une maîtrise des technologies modernes et des bonnes pratiques de développement.

---

[⬅️ Chapitre 15](./chapitre-15-conception-projet-diplome.md) | [➡️ Partie VII](../partie-7-annexes/README.md)

