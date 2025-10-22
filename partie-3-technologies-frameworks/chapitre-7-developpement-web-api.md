# Chapitre 7 : Développement web et API

> **Objectif:** Maîtriser les architectures client-serveur, le développement d'API REST et GraphQL, les frameworks backend modernes et la sécurité des applications web.

---

## 📋 Table des matières

1. [Architecture client-serveur et REST](#71-architecture-client-serveur-et-rest)
2. [Frameworks backend](#72-frameworks-backend-spring-laravel-express-django)
3. [API RESTful et GraphQL](#73-api-restful-et-graphql)
4. [Sécurité et authentification](#74-sécurité-et-authentification-jwt-oauth2-openid-connect)

---

## 7.1 Architecture client-serveur et REST

### 7.1.1 Modèle client-serveur

```
┌─────────────┐                    ┌─────────────┐
│   CLIENT    │─────Request────────►│   SERVER    │
│             │                     │             │
│  - Browser  │◄────Response────────│  - API      │
│  - Mobile   │                     │  - Business │
│  - Desktop  │                     │  - Database │
└─────────────┘                     └─────────────┘
```

**Principes:**
- **Séparation des responsabilités:** Client (présentation) / Serveur (logique + données)
- **Communication:** HTTP/HTTPS
- **Stateless:** Chaque requête contient toute l'information nécessaire
- **Scalabilité:** Client et serveur évoluent indépendamment

### 7.1.2 REST (Representational State Transfer)

**Principes REST:**

1. **Architecture client-serveur**
2. **Sans état (Stateless)**
3. **Cacheable**
4. **Interface uniforme**
5. **Système en couches**
6. **Code à la demande (optionnel)**

**Méthodes HTTP:**

```
GET     /users          Lire la liste des utilisateurs
GET     /users/123      Lire l'utilisateur 123
POST    /users          Créer un nouvel utilisateur
PUT     /users/123      Remplacer l'utilisateur 123
PATCH   /users/123      Modifier partiellement l'utilisateur 123
DELETE  /users/123      Supprimer l'utilisateur 123
```

**Codes de statut HTTP:**

```
2xx Success
200 OK                  Requête réussie
201 Created             Ressource créée
204 No Content          Succès sans contenu (après DELETE)

3xx Redirection
301 Moved Permanently   Ressource déplacée définitivement
304 Not Modified        Ressource non modifiée (cache valide)

4xx Client Errors
400 Bad Request         Requête mal formée
401 Unauthorized        Authentification requise
403 Forbidden           Accès interdit
404 Not Found           Ressource non trouvée
422 Unprocessable Entity Validation échouée

5xx Server Errors
500 Internal Server Error  Erreur serveur
502 Bad Gateway           Erreur de passerelle
503 Service Unavailable   Service temporairement indisponible
```

**Exemple d'API REST bien conçue:**

```javascript
// GET /api/v1/users?page=1&limit=20
{
  "data": [
    {
      "id": 1,
      "email": "john@example.com",
      "name": "John Doe",
      "createdAt": "2024-01-15T10:30:00Z",
      "_links": {
        "self": "/api/v1/users/1",
        "orders": "/api/v1/users/1/orders"
      }
    }
  ],
  "pagination": {
    "currentPage": 1,
    "totalPages": 10,
    "totalItems": 200,
    "itemsPerPage": 20
  },
  "_links": {
    "self": "/api/v1/users?page=1&limit=20",
    "next": "/api/v1/users?page=2&limit=20",
    "last": "/api/v1/users?page=10&limit=20"
  }
}

// POST /api/v1/users
// Request
{
  "email": "jane@example.com",
  "name": "Jane Smith",
  "password": "securePassword123"
}

// Response 201 Created
{
  "id": 201,
  "email": "jane@example.com",
  "name": "Jane Smith",
  "createdAt": "2024-10-21T14:30:00Z",
  "_links": {
    "self": "/api/v1/users/201"
  }
}

// GET /api/v1/users/999 (non-existent)
// Response 404 Not Found
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User with ID 999 not found",
    "timestamp": "2024-10-21T14:35:00Z"
  }
}
```

---

## 7.2 Frameworks backend (Spring, Laravel, Express, Django)

### 7.2.1 Spring Boot (Java)

**Configuration:**

```java
// Application.java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

// Controller
@RestController
@RequestMapping("/api/users")
@CrossOrigin(origins = "http://localhost:3000")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping
    public ResponseEntity<Page<UserDTO>> getUsers(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size) {
        
        Pageable pageable = PageRequest.of(page, size);
        Page<UserDTO> users = userService.findAll(pageable);
        return ResponseEntity.ok(users);
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<UserDTO> getUserById(@PathVariable Long id) {
        return userService.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
    
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public UserDTO createUser(@Valid @RequestBody CreateUserRequest request) {
        return userService.create(request);
    }
    
    @PutMapping("/{id}")
    public ResponseEntity<UserDTO> updateUser(
            @PathVariable Long id,
            @Valid @RequestBody UpdateUserRequest request) {
        
        return userService.update(id, request)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
    
    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}

// Service
@Service
@Transactional
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private PasswordEncoder passwordEncoder;
    
    public Page<UserDTO> findAll(Pageable pageable) {
        return userRepository.findAll(pageable)
            .map(UserMapper::toDTO);
    }
    
    public Optional<UserDTO> findById(Long id) {
        return userRepository.findById(id)
            .map(UserMapper::toDTO);
    }
    
    public UserDTO create(CreateUserRequest request) {
        User user = new User();
        user.setEmail(request.getEmail());
        user.setName(request.getName());
        user.setPassword(passwordEncoder.encode(request.getPassword()));
        
        User saved = userRepository.save(user);
        return UserMapper.toDTO(saved);
    }
}

// Repository
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    boolean existsByEmail(String email);
}

// Configuration
@Configuration
public class WebSecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .requestMatchers("/api/**").authenticated()
            )
            .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS);
        
        return http.build();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

### 7.2.2 Express.js (Node.js)

```javascript
// app.js
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');

const app = express();

// Middleware
app.use(cors());
app.use(helmet());
app.use(morgan('combined'));
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Routes
app.use('/api/auth', require('./routes/auth'));
app.use('/api/users', require('./routes/users'));
app.use('/api/products', require('./routes/products'));

// Error handling
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(err.status || 500).json({
        error: {
            message: err.message,
            ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
        }
    });
});

module.exports = app;

// routes/users.js
const express = require('express');
const router = express.Router();
const { authenticate } = require('../middleware/auth');
const { validate } = require('../middleware/validation');
const userController = require('../controllers/userController');
const { createUserSchema, updateUserSchema } = require('../schemas/userSchemas');

router.get('/', authenticate, userController.getAll);
router.get('/:id', authenticate, userController.getById);
router.post('/', validate(createUserSchema), userController.create);
router.put('/:id', authenticate, validate(updateUserSchema), userController.update);
router.delete('/:id', authenticate, userController.delete);

module.exports = router;

// controllers/userController.js
const userService = require('../services/userService');

exports.getAll = async (req, res, next) => {
    try {
        const { page = 1, limit = 20 } = req.query;
        const result = await userService.findAll(parseInt(page), parseInt(limit));
        res.json(result);
    } catch (error) {
        next(error);
    }
};

exports.getById = async (req, res, next) => {
    try {
        const user = await userService.findById(req.params.id);
        if (!user) {
            return res.status(404).json({ error: { message: 'User not found' } });
        }
        res.json(user);
    } catch (error) {
        next(error);
    }
};

exports.create = async (req, res, next) => {
    try {
        const user = await userService.create(req.body);
        res.status(201).json(user);
    } catch (error) {
        next(error);
    }
};

// services/userService.js
const bcrypt = require('bcrypt');
const User = require('../models/User');

exports.findAll = async (page, limit) => {
    const offset = (page - 1) * limit;
    const users = await User.findAndCountAll({
        limit,
        offset,
        attributes: { exclude: ['password'] }
    });
    
    return {
        data: users.rows,
        pagination: {
            currentPage: page,
            totalPages: Math.ceil(users.count / limit),
            totalItems: users.count,
            itemsPerPage: limit
        }
    };
};

exports.create = async (data) => {
    const hashedPassword = await bcrypt.hash(data.password, 10);
    const user = await User.create({
        ...data,
        password: hashedPassword
    });
    
    const { password, ...userWithoutPassword } = user.toJSON();
    return userWithoutPassword;
};

// middleware/validation.js
const Joi = require('joi');

exports.validate = (schema) => {
    return (req, res, next) => {
        const { error } = schema.validate(req.body, { abortEarly: false });
        if (error) {
            const errors = error.details.map(detail => ({
                field: detail.path.join('.'),
                message: detail.message
            }));
            return res.status(422).json({ errors });
        }
        next();
    };
};

// schemas/userSchemas.js
const Joi = require('joi');

exports.createUserSchema = Joi.object({
    email: Joi.string().email().required(),
    name: Joi.string().min(2).max(100).required(),
    password: Joi.string().min(8).required()
});

exports.updateUserSchema = Joi.object({
    email: Joi.string().email(),
    name: Joi.string().min(2).max(100),
    password: Joi.string().min(8)
}).min(1);
```

### 7.2.3 Laravel (PHP)

```php
// routes/api.php
Route::middleware('auth:sanctum')->group(function () {
    Route::apiResource('users', UserController::class);
    Route::apiResource('products', ProductController::class);
});

// app/Http/Controllers/UserController.php
namespace App\Http\Controllers;

use App\Http\Requests\StoreUserRequest;
use App\Http\Requests\UpdateUserRequest;
use App\Http\Resources\UserResource;
use App\Models\User;
use Illuminate\Http\Request;

class UserController extends Controller
{
    public function index(Request $request)
    {
        $users = User::paginate($request->input('per_page', 20));
        return UserResource::collection($users);
    }
    
    public function show(User $user)
    {
        return new UserResource($user);
    }
    
    public function store(StoreUserRequest $request)
    {
        $user = User::create([
            'name' => $request->name,
            'email' => $request->email,
            'password' => bcrypt($request->password),
        ]);
        
        return new UserResource($user);
    }
    
    public function update(UpdateUserRequest $request, User $user)
    {
        $user->update($request->validated());
        return new UserResource($user);
    }
    
    public function destroy(User $user)
    {
        $user->delete();
        return response()->noContent();
    }
}

// app/Http/Requests/StoreUserRequest.php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreUserRequest extends FormRequest
{
    public function rules()
    {
        return [
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|string|min:8',
        ];
    }
}

// app/Http/Resources/UserResource.php
namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\JsonResource;

class UserResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'email' => $this->email,
            'created_at' => $this->created_at->toIso8601String(),
            '_links' => [
                'self' => route('users.show', $this->id),
            ]
        ];
    }
}
```

### 7.2.4 Django REST Framework (Python)

```python
# urls.py
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import UserViewSet

router = DefaultRouter()
router.register(r'users', UserViewSet)

urlpatterns = [
    path('api/', include(router.urls)),
]

# views.py
from rest_framework import viewsets, status
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated
from .models import User
from .serializers import UserSerializer

class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    permission_classes = [IsAuthenticated]
    
    def get_queryset(self):
        queryset = User.objects.all()
        email = self.request.query_params.get('email')
        if email:
            queryset = queryset.filter(email=email)
        return queryset
    
    @action(detail=True, methods=['post'])
    def set_password(self, request, pk=None):
        user = self.get_object()
        serializer = PasswordSerializer(data=request.data)
        if serializer.is_valid():
            user.set_password(serializer.validated_data['password'])
            user.save()
            return Response({'status': 'password set'})
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

# serializers.py
from rest_framework import serializers
from .models import User

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'email', 'name', 'created_at']
        read_only_fields = ['id', 'created_at']
    
    def create(self, validated_data):
        password = validated_data.pop('password')
        user = User(**validated_data)
        user.set_password(password)
        user.save()
        return user
```

---

## 7.3 API RESTful et GraphQL

### 7.3.1 Conception d'API REST

**Bonnes pratiques:**

```
✅ Utiliser des noms au pluriel
   /users, /products, /orders

✅ Utiliser des noms, pas des verbes
   GET /users/123, pas GET /getUser/123

✅ Utiliser la hiérarchie pour les relations
   GET /users/123/orders
   POST /users/123/orders

✅ Versioning
   /api/v1/users
   Header: Accept: application/vnd.myapi.v2+json

✅ Filtrage, tri, pagination
   GET /users?role=admin&sort=-created_at&page=2&limit=50

✅ Gestion des erreurs cohérente
   {
     "error": {
       "code": "VALIDATION_ERROR",
       "message": "Email already exists",
       "details": [...]
     }
   }

❌ Mauvaises pratiques
   GET /getAllUsers
   POST /user/create
   GET /users/deleteUser/123
```

### 7.3.2 GraphQL

**Principe:** Le client demande exactement les données nécessaires.

**Schéma GraphQL:**

```graphql
type User {
  id: ID!
  email: String!
  name: String!
  orders: [Order!]!
  createdAt: DateTime!
}

type Order {
  id: ID!
  total: Float!
  status: OrderStatus!
  items: [OrderItem!]!
  user: User!
  createdAt: DateTime!
}

enum OrderStatus {
  PENDING
  PAID
  SHIPPED
  DELIVERED
  CANCELLED
}

type Query {
  user(id: ID!): User
  users(page: Int, limit: Int): UserConnection!
  orders(userId: ID, status: OrderStatus): [Order!]!
}

type Mutation {
  createUser(input: CreateUserInput!): User!
  updateUser(id: ID!, input: UpdateUserInput!): User!
  deleteUser(id: ID!): Boolean!
  createOrder(input: CreateOrderInput!): Order!
}

input CreateUserInput {
  email: String!
  name: String!
  password: String!
}

type UserConnection {
  edges: [UserEdge!]!
  pageInfo: PageInfo!
  totalCount: Int!
}
```

**Requête GraphQL:**

```graphql
# Query: récupérer un utilisateur avec ses commandes
query GetUserWithOrders($userId: ID!) {
  user(id: $userId) {
    id
    name
    email
    orders {
      id
      total
      status
      items {
        product {
          name
          price
        }
        quantity
      }
    }
  }
}

# Variables
{
  "userId": "123"
}

# Response
{
  "data": {
    "user": {
      "id": "123",
      "name": "John Doe",
      "email": "john@example.com",
      "orders": [
        {
          "id": "456",
          "total": 129.99,
          "status": "DELIVERED",
          "items": [
            {
              "product": {
                "name": "Laptop",
                "price": 1299.99
              },
              "quantity": 1
            }
          ]
        }
      ]
    }
  }
}

# Mutation: créer un utilisateur
mutation CreateUser($input: CreateUserInput!) {
  createUser(input: $input) {
    id
    email
    name
    createdAt
  }
}

# Variables
{
  "input": {
    "email": "jane@example.com",
    "name": "Jane Smith",
    "password": "securePass123"
  }
}
```

**Implémentation Node.js (Apollo Server):**

```javascript
const { ApolloServer, gql } = require('apollo-server');

// Schema
const typeDefs = gql`
  type User {
    id: ID!
    email: String!
    name: String!
    orders: [Order!]!
  }
  
  type Order {
    id: ID!
    total: Float!
    status: OrderStatus!
    user: User!
  }
  
  enum OrderStatus {
    PENDING
    PAID
    SHIPPED
    DELIVERED
  }
  
  type Query {
    user(id: ID!): User
    users: [User!]!
  }
  
  type Mutation {
    createUser(email: String!, name: String!, password: String!): User!
  }
`;

// Resolvers
const resolvers = {
  Query: {
    user: async (parent, { id }, context) => {
      return context.dataSources.userAPI.getUserById(id);
    },
    users: async (parent, args, context) => {
      return context.dataSources.userAPI.getAllUsers();
    },
  },
  
  Mutation: {
    createUser: async (parent, { email, name, password }, context) => {
      return context.dataSources.userAPI.createUser({ email, name, password });
    },
  },
  
  User: {
    orders: async (user, args, context) => {
      return context.dataSources.orderAPI.getOrdersByUserId(user.id);
    },
  },
  
  Order: {
    user: async (order, args, context) => {
      return context.dataSources.userAPI.getUserById(order.userId);
    },
  },
};

// Server
const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: ({ req }) => ({
    // Context partagé pour tous les resolvers
    user: req.user,
    dataSources: {
      userAPI: new UserAPI(),
      orderAPI: new OrderAPI(),
    },
  }),
});

server.listen().then(({ url }) => {
  console.log(`🚀 Server ready at ${url}`);
});
```

**REST vs GraphQL:**

| Critère | REST | GraphQL |
|---------|------|---------|
| **Flexibilité** | Endpoints fixes | Client choisit les données |
| **Over-fetching** | Oui | Non |
| **Under-fetching** | Oui (multiple requests) | Non |
| **Versioning** | Nécessaire | Évolution du schéma |
| **Caching** | HTTP cache natif | Plus complexe |
| **Courbe d'apprentissage** | Faible | Moyenne |
| **Tooling** | Mature | En croissance |
| **Use cases** | APIs simples, publiques | Apps complexes, clients variés |

---

## 7.4 Sécurité et authentification (JWT, OAuth2, OpenID Connect)

### 7.4.1 JWT (JSON Web Token)

**Structure:**

```
header.payload.signature

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

# Décodé:
Header:
{
  "alg": "HS256",
  "typ": "JWT"
}

Payload:
{
  "sub": "1234567890",
  "name": "John Doe",
  "email": "john@example.com",
  "role": "admin",
  "iat": 1516239022,
  "exp": 1516242622
}

Signature:
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

**Implémentation:**

```javascript
// Node.js avec jsonwebtoken
const jwt = require('jsonwebtoken');

// Générer un token
function generateToken(user) {
    const payload = {
        sub: user.id,
        email: user.email,
        role: user.role
    };
    
    const options = {
        expiresIn: '24h',
        issuer: 'myapp.com'
    };
    
    return jwt.sign(payload, process.env.JWT_SECRET, options);
}

// Vérifier un token
function verifyToken(token) {
    try {
        return jwt.verify(token, process.env.JWT_SECRET);
    } catch (error) {
        if (error.name === 'TokenExpiredError') {
            throw new Error('Token expired');
        }
        throw new Error('Invalid token');
    }
}

// Middleware d'authentification
function authenticate(req, res, next) {
    const authHeader = req.headers.authorization;
    
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
        return res.status(401).json({ error: 'No token provided' });
    }
    
    const token = authHeader.substring(7);
    
    try {
        const decoded = verifyToken(token);
        req.user = decoded;
        next();
    } catch (error) {
        return res.status(401).json({ error: error.message });
    }
}

// Usage
app.post('/api/login', async (req, res) => {
    const { email, password } = req.body;
    
    const user = await User.findByEmail(email);
    if (!user || !await user.verifyPassword(password)) {
        return res.status(401).json({ error: 'Invalid credentials' });
    }
    
    const token = generateToken(user);
    res.json({ token, user: { id: user.id, email: user.email } });
});

app.get('/api/profile', authenticate, (req, res) => {
    res.json({ user: req.user });
});
```

### 7.4.2 OAuth 2.0

**Flux Authorization Code:**

```
1. User ─────────────► Authorization Server
   (clicks "Login with Google")

2. Authorization Server ───► User
   (redirects to consent page)

3. User ─────────────► Authorization Server
   (grants permission)

4. Authorization Server ───► Client App
   (redirects with authorization code)

5. Client App ────────► Authorization Server
   (exchanges code for access token)

6. Authorization Server ───► Client App
   (returns access token + refresh token)

7. Client App ────────► Resource Server
   (requests data with access token)

8. Resource Server ───────► Client App
   (returns protected data)
```

**Implémentation (Passport.js avec Google OAuth):**

```javascript
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: '/auth/google/callback'
  },
  async (accessToken, refreshToken, profile, done) => {
    try {
      // Trouver ou créer l'utilisateur
      let user = await User.findOne({ googleId: profile.id });
      
      if (!user) {
        user = await User.create({
          googleId: profile.id,
          email: profile.emails[0].value,
          name: profile.displayName,
          avatar: profile.photos[0].value
        });
      }
      
      return done(null, user);
    } catch (error) {
      return done(error, null);
    }
  }
));

// Routes
app.get('/auth/google',
  passport.authenticate('google', { scope: ['profile', 'email'] })
);

app.get('/auth/google/callback',
  passport.authenticate('google', { failureRedirect: '/login' }),
  (req, res) => {
    // Générer notre propre JWT
    const token = generateToken(req.user);
    res.redirect(`/dashboard?token=${token}`);
  }
);
```

### 7.4.3 HTTPS et sécurité des communications

**Certificat SSL/TLS:**

```bash
# Générer un certificat auto-signé (dev)
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes

# Node.js avec HTTPS
const https = require('https');
const fs = require('fs');
const express = require('express');

const app = express();

const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem')
};

https.createServer(options, app).listen(443);
```

**Headers de sécurité (Helmet.js):**

```javascript
const helmet = require('helmet');

app.use(helmet());  // Active tous les headers de sécurité

// Ou configuration spécifique:
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"],
    },
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  }
}));

// Headers ajoutés:
// X-Content-Type-Options: nosniff
// X-Frame-Options: DENY
// X-XSS-Protection: 1; mode=block
// Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
// Content-Security-Policy: ...
```

**Rate Limiting:**

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Max 100 requêtes par IP
  message: 'Too many requests, please try again later.'
});

app.use('/api/', limiter);

// Rate limiting spécifique pour login
const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5, // Max 5 tentatives de connexion
  skipSuccessfulRequests: true
});

app.post('/api/login', loginLimiter, loginController);
```

---

## 📝 Résumé du chapitre

✅ **REST:** Architecture basée sur HTTP avec ressources et méthodes standard

✅ **Frameworks:** Spring (Java), Express (Node.js), Laravel (PHP), Django (Python)

✅ **GraphQL:** Alternative à REST permettant au client de définir exactement les données nécessaires

✅ **JWT:** Tokens stateless pour authentification

✅ **OAuth 2.0:** Délégation d'authentification à des providers tiers

✅ **Sécurité:** HTTPS, headers sécurisés, rate limiting

---

## 💡 Exercices pratiques

### Exercice 1
Créer une API REST complète pour un système de blog (articles, commentaires, utilisateurs) avec authentification JWT.

### Exercice 2
Implémenter une API GraphQL pour le même système et comparer avec REST.

### Exercice 3
Intégrer OAuth 2.0 avec Google et GitHub pour l'authentification.

---

## 📚 Pour aller plus loin

- 📖 **"RESTful Web APIs"** - Leonard Richardson
- 📖 **"GraphQL in Action"** - Samer Buna
- 🔗 **jwt.io**: Debugger JWT
- 🔗 **OAuth.net**: Documentation OAuth 2.0

---

[⬅️ Partie III](./README.md) | [➡️ Chapitre 8](./chapitre-8-developpement-desktop-mobile.md)

