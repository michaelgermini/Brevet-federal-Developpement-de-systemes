# Chapitre 3 : Architecture logicielle

> **Objectif:** Maîtriser les principes de conception d'architectures logicielles, les modèles d'architecture modernes et les patterns architecturaux essentiels.

---

## 📋 Table des matières

1. [Principes de conception d'architectures logicielles](#31-principes-de-conception-darchitectures-logicielles)
2. [Modèles d'architecture](#32-modèles-darchitecture-monolithique-microservices-soa)
3. [Patterns architecturaux](#33-patterns-architecturaux-mvc-cqrs-event-driven-layered)
4. [Outils d'architecture](#34-outils-darchitecture-diagrammes-modélisation-et-documentation)

---

## 3.1 Principes de conception d'architectures logicielles

### 3.1.1 Qu'est-ce qu'une architecture logicielle?

**Définition:**
> L'architecture logicielle est l'organisation fondamentale d'un système, incarnée dans ses composants, leurs relations mutuelles et avec l'environnement, et les principes guidant sa conception et son évolution.

**Rôle de l'architecte logiciel:**
- 🎯 Définir la structure globale du système
- ⚖️ Arbitrer entre contraintes techniques et besoins métier
- 🔍 Anticiper les évolutions et la scalabilité
- 🛡️ Garantir qualité, sécurité et performance
- 📚 Documenter et communiquer les décisions

### 3.1.2 Qualités architecturales (ISO 25010)

**Caractéristiques de qualité:**

```
┌─────────────────────────────────────────────┐
│         QUALITÉS EXTERNES                   │
│  (visibles par les utilisateurs)            │
├─────────────────────────────────────────────┤
│ • Fonctionnalité: répond aux besoins        │
│ • Performance: temps de réponse, débit      │
│ • Utilisabilité: facilité d'utilisation     │
│ • Fiabilité: disponibilité, tolérance pannes│
│ • Sécurité: confidentialité, intégrité      │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│         QUALITÉS INTERNES                   │
│  (visibles par les développeurs)            │
├─────────────────────────────────────────────┤
│ • Maintenabilité: facilité de modification  │
│ • Testabilité: facilité de tests            │
│ • Portabilité: adaptation à environnements  │
│ • Réutilisabilité: composants réutilisables │
│ • Modularité: découpage cohérent            │
└─────────────────────────────────────────────┘
```

**Trade-offs (compromis):**
```
Performance  ⟷  Maintenabilité
    ↕              ↕
Sécurité   ⟷  Facilité d'utilisation
    ↕              ↕
Rapidité   ⟷  Qualité du code
développement
```

### 3.1.3 Principes fondamentaux

#### Séparation des préoccupations (Separation of Concerns)

**Principe:** Diviser le système en parties distinctes avec responsabilités spécifiques.

```
Application e-commerce:
├── Présentation (UI)
│   └── Responsabilité: Afficher les données, interactions utilisateur
├── Logique métier (Business Logic)
│   └── Responsabilité: Règles métier, calculs, validations
├── Accès aux données (Data Access)
│   └── Responsabilité: Communication avec la base de données
└── Infrastructure
    └── Responsabilité: Logging, sécurité, caching
```

#### Abstraction et encapsulation

**Abstraction:** Cacher les détails d'implémentation, exposer l'essentiel.

```java
// Interface abstraite (contrat)
public interface PaymentProcessor {
    PaymentResult process(Payment payment);
}

// Implémentations concrètes cachées
public class StripePaymentProcessor implements PaymentProcessor {
    // Détails spécifiques à Stripe
}

public class PayPalPaymentProcessor implements PaymentProcessor {
    // Détails spécifiques à PayPal
}

// Utilisation: le client ne connaît que l'interface
PaymentProcessor processor = paymentFactory.getProcessor(paymentType);
PaymentResult result = processor.process(payment);
```

#### Couplage faible, cohésion forte

**Couplage faible:** Minimiser les dépendances entre modules.
**Cohésion forte:** Regrouper ce qui change ensemble.

```
❌ Couplage fort:
OrderService ──dépend──> MySQLDatabase
                └──> Impossible de changer de DB

✅ Couplage faible:
OrderService ──utilise──> IOrderRepository (interface)
                              ↑
                   MySQLOrderRepository implémente
```

#### Principe de substitution (Liskov)

**Principe:** Les sous-types doivent pouvoir remplacer leurs types de base.

```java
// Type de base
public abstract class NotificationService {
    public abstract void send(String message, String recipient);
}

// Sous-types substituables
public class EmailNotificationService extends NotificationService {
    @Override
    public void send(String message, String recipient) {
        // Envoi par email
    }
}

public class SMSNotificationService extends NotificationService {
    @Override
    public void send(String message, String recipient) {
        // Envoi par SMS
    }
}

// Utilisation polymorphique
NotificationService notif = getNotificationService();
notif.send("Votre commande est expédiée", user.getContact());
```

#### DRY (Don't Repeat Yourself)

**Principe:** Chaque connaissance doit avoir une représentation unique.

```java
❌ Violation de DRY:
public class OrderController {
    public void createOrder() {
        if (user.getAge() < 18 || user.isBlacklisted()) {
            throw new Exception("Non autorisé");
        }
        // ...
    }
}

public class SubscriptionController {
    public void subscribe() {
        if (user.getAge() < 18 || user.isBlacklisted()) {
            throw new Exception("Non autorisé");
        }
        // ...
    }
}

✅ Respect de DRY:
public class AuthorizationService {
    public void checkEligibility(User user) {
        if (user.getAge() < 18 || user.isBlacklisted()) {
            throw new UnauthorizedException();
        }
    }
}

// Utilisation
authService.checkEligibility(user);
```

---

## 3.2 Modèles d'architecture (monolithique, microservices, SOA)

### 3.2.1 Architecture monolithique

**Caractéristiques:**
- Application unique et indivisible
- Déployée comme une seule unité
- Base de données centralisée

```
┌─────────────────────────────────────────┐
│      APPLICATION MONOLITHIQUE           │
│                                         │
│  ┌──────────────────────────────────┐  │
│  │         Interface Web            │  │
│  └──────────────┬───────────────────┘  │
│                 │                       │
│  ┌──────────────▼───────────────────┐  │
│  │      Logique Métier              │  │
│  │  ├── Module Utilisateurs         │  │
│  │  ├── Module Produits             │  │
│  │  ├── Module Commandes            │  │
│  │  └── Module Paiements            │  │
│  └──────────────┬───────────────────┘  │
│                 │                       │
│  ┌──────────────▼───────────────────┐  │
│  │      Accès aux données           │  │
│  └──────────────┬───────────────────┘  │
└─────────────────┼───────────────────────┘
                  │
         ┌────────▼─────────┐
         │   Base données   │
         │   (PostgreSQL)   │
         └──────────────────┘
```

**✅ Avantages:**
- Simplicité de développement initial
- Facilité de déploiement (un seul package)
- Debugging plus simple
- Pas de latence réseau inter-composants
- Transactions ACID simples

**❌ Inconvénients:**
- Scalabilité limitée (scaling vertical uniquement)
- Couplage fort entre modules
- Déploiement risqué (tout ou rien)
- Difficulté à adopter nouvelles technologies
- Temps de build et démarrage longs
- Équipe doit maîtriser tout le stack

**🎯 Quand utiliser:**
- MVP ou petit projet
- Équipe réduite
- Domaine métier simple
- Contraintes de latence strictes

### 3.2.2 Architecture orientée services (SOA)

**Caractéristiques:**
- Services métier réutilisables
- Communication via bus d'entreprise (ESB)
- Standardisation (SOAP, WSDL)

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│   Portal     │    │  Application │    │   Mobile     │
│     Web      │    │   Desktop    │    │     App      │
└──────┬───────┘    └──────┬───────┘    └──────┬───────┘
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
              ┌────────────▼────────────┐
              │  Enterprise Service Bus │
              │         (ESB)           │
              └────────────┬────────────┘
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
┌──────▼───────┐    ┌──────▼───────┐   ┌──────▼───────┐
│   Service    │    │   Service    │   │   Service    │
│ Utilisateurs │    │   Produits   │   │  Commandes   │
└──────────────┘    └──────────────┘   └──────────────┘
```

**✅ Avantages:**
- Réutilisation des services
- Interopérabilité (standards)
- Gouvernance centralisée
- Intégration legacy facilitée

**❌ Inconvénients:**
- Complexité de l'ESB (point de défaillance)
- Performance (overhead XML/SOAP)
- Couplage via schémas partagés
- Gouvernance lourde

**🎯 Quand utiliser:**
- Grande entreprise avec SI hétérogène
- Besoin d'intégration legacy
- Exigences de gouvernance strictes

### 3.2.3 Architecture microservices

**Caractéristiques:**
- Services autonomes, déployables indépendamment
- Communication légère (REST, messages)
- Base de données par service
- Organisation par domaine métier

```
┌────────────────────────────────────────────────────────┐
│                     API Gateway                        │
└─────┬──────────┬──────────┬──────────┬─────────┬──────┘
      │          │          │          │         │
┌─────▼────┐ ┌──▼─────┐ ┌──▼─────┐ ┌──▼─────┐ ┌▼──────┐
│  User    │ │Product │ │ Order  │ │Payment │ │Notif. │
│ Service  │ │Service │ │Service │ │Service │ │Service│
├──────────┤ ├────────┤ ├────────┤ ├────────┤ ├───────┤
│  DB      │ │  DB    │ │  DB    │ │  DB    │ │  MQ   │
│(Postgres)│ │(Mongo) │ │(MySQL) │ │(Postgres)│(Rabbit)│
└──────────┘ └────────┘ └────────┘ └────────┘ └───────┘

     Event Bus (Kafka / RabbitMQ)
     ════════════════════════════════
     OrderCreated → Payment, Notification
     PaymentReceived → Order, Warehouse
```

**✅ Avantages:**
- Scalabilité indépendante par service
- Déploiement indépendant (réduction risque)
- Technologie adaptée par service
- Équipes autonomes (ownership)
- Résilience (isolation des pannes)
- Facilite CI/CD

**❌ Inconvénients:**
- Complexité opérationnelle (orchestration)
- Transactions distribuées complexes
- Latence réseau
- Debugging difficile (traces distribuées)
- Duplication de code/logique
- Nécessite DevOps mature

**🎯 Quand utiliser:**
- Application complexe à forte charge
- Équipes multiples
- Besoin de scalabilité différenciée
- Évolution rapide requise
- Organisation DevOps mature

### 3.2.4 Architecture serverless

**Caractéristiques:**
- Fonctions éphémères (FaaS)
- Scaling automatique
- Paiement à l'usage
- Gestion infrastructure par provider

```
┌────────────────────────────────────────┐
│         Client (Web/Mobile)            │
└──────────────┬─────────────────────────┘
               │
┌──────────────▼─────────────────────────┐
│         API Gateway (AWS/Azure)        │
└──┬────────┬────────┬────────┬──────────┘
   │        │        │        │
┌──▼───┐ ┌──▼───┐ ┌──▼───┐ ┌──▼────┐
│Lambda│ │Lambda│ │Lambda│ │Lambda │
│User  │ │Order │ │Payment│ │Notif  │
└──┬───┘ └──┬───┘ └──┬───┘ └──┬────┘
   │        │        │        │
┌──▼───┐ ┌──▼───┐ ┌──▼───┐ ┌──▼────┐
│DynamoDB S3   │RDS   │SQS    │
└──────┘ └──────┘ └──────┘ └───────┘
```

**✅ Avantages:**
- Coût optimisé (pay-per-use)
- Scaling automatique infini
- Pas de gestion serveurs
- Time-to-market rapide

**❌ Inconvénients:**
- Cold start (latence initiale)
- Limites d'exécution (temps, mémoire)
- Vendor lock-in
- Debugging complexe
- Pas adapté aux traitements longs

**🎯 Quand utiliser:**
- Traitement événementiel
- Pics de charge imprévisibles
- Budget limité
- Prototypes rapides

### 3.2.5 Comparaison

| Critère | Monolithe | SOA | Microservices | Serverless |
|---------|-----------|-----|---------------|------------|
| **Complexité** | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |
| **Scalabilité** | ⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Performance** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| **Coût initial** | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐ |
| **Maintenance** | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Déploiement** | ⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

---

## 3.3 Patterns architecturaux (MVC, CQRS, Event-driven, Layered)

### 3.3.1 Architecture en couches (Layered Architecture)

**Structure classique (4 couches):**

```
┌─────────────────────────────────────────┐
│     Couche Présentation (UI)            │
│  - Controllers / Views / APIs           │
│  - Validation des entrées               │
└──────────────┬──────────────────────────┘
               │DTO
┌──────────────▼──────────────────────────┐
│     Couche Application (Services)       │
│  - Orchestration des use cases          │
│  - Logique applicative                  │
└──────────────┬──────────────────────────┘
               │Entities
┌──────────────▼──────────────────────────┐
│     Couche Domaine (Business Logic)     │
│  - Entités métier                       │
│  - Règles de gestion                    │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│     Couche Infrastructure               │
│  - Repositories                         │
│  - Accès base de données                │
│  - Services externes                    │
└─────────────────────────────────────────┘
```

**Règles:**
- Chaque couche ne peut appeler que la couche inférieure
- Pas de saut de couche (Présentation → Infrastructure interdit)
- Les dépendances vont vers le bas

**Exemple concret:**

```java
// Couche Présentation
@RestController
@RequestMapping("/api/orders")
public class OrderController {
    @Autowired
    private OrderService orderService;
    
    @PostMapping
    public ResponseEntity<OrderDTO> createOrder(@RequestBody CreateOrderRequest request) {
        OrderDTO order = orderService.createOrder(request);
        return ResponseEntity.ok(order);
    }
}

// Couche Application
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;
    
    @Autowired
    private PaymentService paymentService;
    
    @Transactional
    public OrderDTO createOrder(CreateOrderRequest request) {
        // Orchestration
        Order order = new Order(request.getUserId(), request.getItems());
        order.calculateTotal();
        
        orderRepository.save(order);
        paymentService.processPayment(order);
        
        return OrderMapper.toDTO(order);
    }
}

// Couche Domaine
public class Order {
    private Long id;
    private List<OrderItem> items;
    private Money total;
    
    public void calculateTotal() {
        // Règle métier
        this.total = items.stream()
            .map(OrderItem::getSubtotal)
            .reduce(Money.ZERO, Money::add);
    }
}

// Couche Infrastructure
@Repository
public class OrderRepositoryImpl implements OrderRepository {
    @PersistenceContext
    private EntityManager entityManager;
    
    @Override
    public void save(Order order) {
        entityManager.persist(order);
    }
}
```

### 3.3.2 MVC (Model-View-Controller)

**Séparation des responsabilités UI:**

```
         ┌───────────┐
         │   USER    │
         └─────┬─────┘
               │ interactions
         ┌─────▼─────────┐
         │     VIEW      │◄──────┐
         │  (Template)   │       │ updates
         └─────┬─────────┘       │
               │ events          │
         ┌─────▼─────────┐       │
         │  CONTROLLER   │───────┤
         │  (Logic)      │       │ read
         └─────┬─────────┘       │
               │ updates         │
         ┌─────▼─────────┐       │
         │     MODEL     │───────┘
         │  (Data)       │
         └───────────────┘
```

**Exemple Spring MVC:**

```java
// Model
@Entity
public class Product {
    @Id
    private Long id;
    private String name;
    private BigDecimal price;
    // getters/setters
}

// Controller
@Controller
@RequestMapping("/products")
public class ProductController {
    @Autowired
    private ProductService productService;
    
    @GetMapping
    public String listProducts(Model model) {
        List<Product> products = productService.getAllProducts();
        model.addAttribute("products", products);
        return "products/list";  // View name
    }
    
    @PostMapping
    public String createProduct(@ModelAttribute Product product) {
        productService.save(product);
        return "redirect:/products";
    }
}

// View (Thymeleaf template: products/list.html)
<table>
    <tr th:each="product : ${products}">
        <td th:text="${product.name}"></td>
        <td th:text="${product.price}"></td>
    </tr>
</table>
```

### 3.3.3 CQRS (Command Query Responsibility Segregation)

**Principe:** Séparer les opérations de lecture (Query) et d'écriture (Command).

```
┌──────────────────────────────────────────────┐
│              Application                     │
└──────┬─────────────────────────┬─────────────┘
       │                         │
   Commands                  Queries
   (Write)                   (Read)
       │                         │
┌──────▼─────────┐      ┌────────▼──────────┐
│ Command Model  │      │   Query Model     │
│                │      │                   │
│ - Rich domain  │      │ - DTOs optimisés  │
│ - Validations  │      │ - Dénormalisées   │
│ - Rules        │      │ - Cache           │
└───────┬────────┘      └────────┬──────────┘
        │                        │
┌───────▼────────┐      ┌────────▼──────────┐
│  Write DB      │      │   Read DB         │
│  (normalized)  │─sync─│ (denormalized)    │
└────────────────┘      └───────────────────┘
```

**Exemple:**

```java
// Command Side
public class CreateOrderCommand {
    private Long userId;
    private List<OrderItemDTO> items;
}

@Service
public class OrderCommandHandler {
    @Autowired
    private OrderRepository orderRepository;
    
    @Autowired
    private EventBus eventBus;
    
    public void handle(CreateOrderCommand command) {
        Order order = new Order(command.getUserId(), command.getItems());
        orderRepository.save(order);
        
        // Publier événement
        eventBus.publish(new OrderCreatedEvent(order.getId()));
    }
}

// Query Side
public class OrderDTO {
    private Long id;
    private String customerName;  // Dénormalisé
    private BigDecimal total;
    private List<OrderItemDTO> items;
}

@Service
public class OrderQueryHandler {
    @Autowired
    private OrderReadRepository readRepository;  // DB read optimisée
    
    public OrderDTO getOrderById(Long id) {
        return readRepository.findOrderDTO(id);
    }
    
    public List<OrderDTO> getOrdersByUser(Long userId) {
        return readRepository.findOrdersByUser(userId);
    }
}

// Synchronisation (Event Handler)
@Component
public class OrderProjection {
    @EventHandler
    public void on(OrderCreatedEvent event) {
        // Mettre à jour le modèle de lecture
        readRepository.createOrderProjection(event);
    }
}
```

**✅ Avantages:**
- Performance optimisée (DB read/write séparées)
- Scalabilité indépendante
- Modèles adaptés à leur usage
- Sécurité accrue (séparation permissions)

**❌ Inconvénients:**
- Complexité accrue
- Cohérence éventuelle (eventual consistency)
- Duplication de données

### 3.3.4 Architecture événementielle (Event-Driven)

**Principe:** Communication asynchrone via événements.

```
┌──────────────┐         Events         ┌──────────────┐
│   Service A  │────────────────────────►│ Event Bus    │
│  (Producer)  │                         │ (Kafka/RMQ)  │
└──────────────┘                         └──────┬───────┘
                                                │
                    ┌───────────────────────────┼───────┐
                    │                           │       │
              ┌─────▼──────┐          ┌─────────▼────┐  │
              │ Service B  │          │  Service C   │  │
              │ (Consumer) │          │  (Consumer)  │  │
              └────────────┘          └──────────────┘  │
                                                        │
                                              ┌─────────▼────┐
                                              │  Service D   │
                                              │  (Consumer)  │
                                              └──────────────┘
```

**Exemple: E-commerce avec événements**

```java
// Événement
public class OrderPlacedEvent {
    private Long orderId;
    private Long userId;
    private BigDecimal amount;
    private LocalDateTime timestamp;
}

// Producer
@Service
public class OrderService {
    @Autowired
    private EventPublisher eventPublisher;
    
    public void placeOrder(Order order) {
        orderRepository.save(order);
        
        // Publier événement
        OrderPlacedEvent event = new OrderPlacedEvent(
            order.getId(),
            order.getUserId(),
            order.getTotal()
        );
        eventPublisher.publish("orders.placed", event);
    }
}

// Consumers (réagissent indépendamment)

// 1. Service de paiement
@Component
public class PaymentEventHandler {
    @EventListener("orders.placed")
    public void handleOrderPlaced(OrderPlacedEvent event) {
        // Initier le paiement
        paymentService.createPaymentRequest(event.getOrderId(), event.getAmount());
    }
}

// 2. Service de notification
@Component
public class NotificationEventHandler {
    @EventListener("orders.placed")
    public void handleOrderPlaced(OrderPlacedEvent event) {
        // Envoyer confirmation email
        emailService.sendOrderConfirmation(event.getUserId(), event.getOrderId());
    }
}

// 3. Service d'inventaire
@Component
public class InventoryEventHandler {
    @EventListener("orders.placed")
    public void handleOrderPlaced(OrderPlacedEvent event) {
        // Réserver le stock
        inventoryService.reserveStock(event.getOrderId());
    }
}
```

**✅ Avantages:**
- Découplage fort
- Scalabilité
- Extensibilité (ajout facile de consumers)
- Résilience (retry, dead-letter queues)

**❌ Inconvénients:**
- Cohérence éventuelle
- Debugging complexe
- Ordre des événements à gérer
- Duplication possible

### 3.3.5 Hexagonal Architecture (Ports & Adapters)

**Principe:** Isoler la logique métier des détails techniques.

```
                  ┌──────────────┐
                  │  REST API    │ (Adapter)
                  └──────┬───────┘
                         │
       ┌─────────────────▼─────────────────┐
       │          Port (Interface)         │
       ├───────────────────────────────────┤
       │                                   │
       │      ┌───────────────────┐        │
       │      │   DOMAIN CORE     │        │
       │      │  (Business Logic) │        │
       │      └───────────────────┘        │
       │                                   │
       ├───────────────────────────────────┤
       │          Port (Interface)         │
       └─────────────────┬─────────────────┘
                         │
         ┌───────────────┼────────────────┐
         │               │                │
   ┌─────▼──────┐ ┌──────▼─────┐ ┌───────▼────┐
   │ PostgreSQL │ │   Redis    │ │  Email     │
   │  (Adapter) │ │  (Adapter) │ │  (Adapter) │
   └────────────┘ └────────────┘ └────────────┘
```

**Exemple:**

```java
// Domain Core (sans dépendances externes)
public class Order {
    private Long id;
    private List<OrderItem> items;
    
    public void addItem(Product product, int quantity) {
        // Pure business logic
        items.add(new OrderItem(product, quantity));
    }
}

// Port (Interface côté domaine)
public interface OrderRepository {
    Order findById(Long id);
    void save(Order order);
}

public interface PaymentGateway {
    PaymentResult process(Payment payment);
}

// Adapter Infrastructure (implémentation technique)
@Repository
public class PostgreSQLOrderRepository implements OrderRepository {
    @PersistenceContext
    private EntityManager em;
    
    @Override
    public Order findById(Long id) {
        return em.find(Order.class, id);
    }
    
    @Override
    public void save(Order order) {
        em.persist(order);
    }
}

// Adapter API (point d'entrée)
@RestController
public class OrderController {
    @Autowired
    private OrderService orderService;  // Use case
    
    @PostMapping("/orders")
    public ResponseEntity<OrderDTO> create(@RequestBody CreateOrderRequest request) {
        Order order = orderService.createOrder(request);
        return ResponseEntity.ok(OrderMapper.toDTO(order));
    }
}
```

**✅ Avantages:**
- Testabilité maximale (domain isolé)
- Changement facile de technologie
- Focus sur le métier
- Évolutivité

---

## 3.4 Outils d'architecture: diagrammes, modélisation et documentation

### 3.4.1 Architecture Decision Records (ADR)

**Format standard:**

```markdown
# ADR-001: Choix de l'architecture microservices

## Statut
Accepté

## Contexte
L'application monolithique actuelle souffre de:
- Déploiements risqués (tout ou rien)
- Scaling impossible par fonctionnalité
- Temps de build > 10 minutes
- Équipes bloquées par dépendances

## Décision
Migrer vers une architecture microservices avec:
- 1 service par bounded context
- Communication async (Kafka)
- API Gateway (Kong)
- DB par service

## Conséquences

Positives:
- Déploiements indépendants
- Scaling granulaire
- Technologie adaptée par service

Négatives:
- Complexité opérationnelle accrue
- Nécessite investissement DevOps
- Transactions distribuées complexes

## Alternatives considérées
- Modularisation du monolithe: insuffisant long terme
- SOA avec ESB: trop lourd, couplage

## Date
2024-10-15
```

### 3.4.2 Diagramme C4 Model

**Niveaux d'abstraction:**

```
Level 1: Context (Vue d'ensemble)
┌─────────────────────────────────────┐
│          E-commerce System          │
└───────────┬─────────────────────────┘
            │
   ┌────────┴─────────┐
   │                  │
┌──▼────┐      ┌──────▼───────┐
│Customer│     │Payment Gateway│
└───────┘      └──────────────┘

Level 2: Container (Applications)
┌────────────────────────────────────┐
│      E-commerce System             │
├────────────────────────────────────┤
│  ┌──────────┐    ┌─────────────┐  │
│  │ Web App  │    │   API       │  │
│  │(React)   │────┤  (Node.js)  │  │
│  └──────────┘    └──────┬──────┘  │
│                         │          │
│                  ┌──────▼──────┐   │
│                  │  Database   │   │
│                  │ (PostgreSQL)│   │
│                  └─────────────┘   │
└────────────────────────────────────┘

Level 3: Component (Modules internes)
Level 4: Code (Classes)
```

### 3.4.3 Outils recommandés

| Outil | Usage | Points forts |
|-------|-------|--------------|
| **PlantUML** | Diagrammes as code | Versionnable, automatisable |
| **Draw.io / Diagrams.net** | Diagrammes visuels | Gratuit, collaboratif |
| **Lucidchart** | Modélisation | Templates, partage |
| **Miro** | Collaboration | Workshops, brainstorming |
| **Structurizr** | C4 Model | Spécialisé architecture |
| **ArchiMate** | Enterprise Architecture | Standard, complet |

---

## 📝 Résumé du chapitre

✅ L'**architecture** définit la structure, les composants et leurs relations

✅ **Qualités architecturales:** performance, sécurité, maintenabilité, scalabilité

✅ **Modèles:** Monolithe (simple), SOA (intégration), Microservices (scalabilité), Serverless (élasticité)

✅ **Patterns:** Layered (séparation), MVC (UI), CQRS (read/write), Event-Driven (asynchrone), Hexagonal (isolation)

✅ **Documentation:** ADR pour décisions, C4 pour visualisation

---

## 💡 Exercices pratiques

### Exercice 1: Choisir l'architecture

Pour une application de streaming vidéo type Netflix, justifiez le choix d'architecture et les patterns.

### Exercice 2: Modéliser

Créez un diagramme C4 (niveau 2) pour un système de réservation de vols.

### Exercice 3: ADR

Rédigez un ADR pour le choix entre base SQL et NoSQL pour un système de logs.

---

## 📚 Pour aller plus loin

- 📖 **"Clean Architecture"** - Robert C. Martin
- 📖 **"Building Microservices"** - Sam Newman
- 📖 **"Software Architecture Patterns"** - Mark Richards
- 🔗 **c4model.com**: The C4 Model for visualizing software architecture

---

[⬅️ Chapitre 2](./chapitre-2-analyse-besoins-specifications.md) | [➡️ Partie II](../partie-2-conception-developpement/README.md)

