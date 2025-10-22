# Chapitre 9 : Bases de données et persistance

> **Objectif:** Maîtriser la modélisation de données, SQL avancé, les ORM, les bases NoSQL et la gestion des migrations pour une persistance efficace et robuste.

---

## 📋 Table des matières

1. [Modélisation relationnelle et normalisation](#91-modélisation-relationnelle-et-normalisation)
2. [SQL avancé et ORM](#92-sql-avancé-et-orm-hibernate-sequelize-prisma)
3. [Bases NoSQL](#93-bases-nosql-mongodb-redis-firebase-cassandra)
4. [Gestion des migrations et intégrité des données](#94-gestion-des-migrations-et-intégrité-des-données)

---

## 9.1 Modélisation relationnelle et normalisation

### 9.1.1 Modèle Entité-Association (MCD/MLD)

**Exemple: Système e-commerce**

```
┌────────────┐        ┌────────────┐        ┌────────────┐
│   USER     │        │   ORDER    │        │  PRODUCT   │
├────────────┤        ├────────────┤        ├────────────┤
│ id         │───┐    │ id         │    ┌───│ id         │
│ email      │   │    │ user_id    │────│   │ name       │
│ name       │   └───►│ total      │    │   │ price      │
│ created_at │        │ status     │    │   │ stock      │
└────────────┘        │ created_at │    │   │ category   │
                      └─────┬──────┘    │   └────────────┘
                            │           │
                            │ 1      N  │
                            ▼           │
                      ┌────────────────┐│
                      │  ORDER_ITEM    ││
                      ├────────────────┤│
                      │ id             ││
                      │ order_id       │◄
                      │ product_id     │◄───┘
                      │ quantity       │
                      │ unit_price     │
                      └────────────────┘

Relations:
- Un USER peut avoir plusieurs ORDERS (1:N)
- Un ORDER contient plusieurs PRODUCTS via ORDER_ITEM (N:M)
- Un PRODUCT peut être dans plusieurs ORDERS via ORDER_ITEM (N:M)
```

**Schéma SQL:**

```sql
-- Table Users
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Index pour recherche rapide
CREATE INDEX idx_users_email ON users(email);

-- Table Products
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL CHECK (price >= 0),
    stock INTEGER NOT NULL DEFAULT 0 CHECK (stock >= 0),
    category VARCHAR(100),
    image_url VARCHAR(500),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_products_category ON products(category);
CREATE INDEX idx_products_price ON products(price);

-- Table Orders
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    total DECIMAL(10, 2) NOT NULL CHECK (total >= 0),
    status VARCHAR(50) NOT NULL DEFAULT 'PENDING',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT check_status CHECK (
        status IN ('PENDING', 'PAID', 'SHIPPED', 'DELIVERED', 'CANCELLED')
    )
);

CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created_at ON orders(created_at);

-- Table Order_Items (table de liaison)
CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    order_id INTEGER NOT NULL REFERENCES orders(id) ON DELETE CASCADE,
    product_id INTEGER NOT NULL REFERENCES products(id) ON DELETE RESTRICT,
    quantity INTEGER NOT NULL CHECK (quantity > 0),
    unit_price DECIMAL(10, 2) NOT NULL CHECK (unit_price >= 0),
    UNIQUE(order_id, product_id)
);

CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_items_product_id ON order_items(product_id);
```

### 9.1.2 Normalisation

**Formes normales:**

**1NF (Première Forme Normale):**
- Attributs atomiques (pas de valeurs multiples)
- Chaque ligne est unique

```sql
❌ Non normalisé:
CREATE TABLE orders (
    id INT,
    user_name VARCHAR(255),
    products VARCHAR(500)  -- "Laptop,Mouse,Keyboard"
);

✅ 1NF:
CREATE TABLE orders (
    id INT,
    user_name VARCHAR(255)
);

CREATE TABLE order_items (
    order_id INT,
    product_name VARCHAR(255)
);
```

**2NF (Deuxième Forme Normale):**
- Respecte 1NF
- Pas de dépendance partielle (attributs dépendent de toute la clé primaire)

```sql
❌ Viole 2NF:
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    product_name VARCHAR(255),  -- Dépend seulement de product_id
    product_price DECIMAL,      -- Dépend seulement de product_id
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);

✅ 2NF:
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL
);

CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

**3NF (Troisième Forme Normale):**
- Respecte 2NF
- Pas de dépendance transitive

```sql
❌ Viole 3NF:
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    user_name VARCHAR(255),     -- Dépendance transitive (via user_id)
    user_email VARCHAR(255),    -- Dépendance transitive
    total DECIMAL
);

✅ 3NF:
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255)
);

CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    total DECIMAL,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### 9.1.3 Dénormalisation stratégique

**Cas d'usage:** Optimisation des lectures au détriment de l'espace

```sql
-- Table normalisée (3NF)
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Chaque lecture nécessite un JOIN
SELECT o.*, u.name, u.email
FROM orders o
JOIN users u ON o.user_id = u.id;

-- Table dénormalisée pour performance
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    user_name VARCHAR(255),     -- Dénormalisé
    user_email VARCHAR(255),    -- Dénormalisé
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Lecture directe, plus rapide
SELECT * FROM orders WHERE id = 123;

-- Compromis: Maintenir la cohérence via triggers
CREATE OR REPLACE FUNCTION update_order_user_info()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE orders
    SET user_name = NEW.name,
        user_email = NEW.email
    WHERE user_id = NEW.id;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER user_update_trigger
AFTER UPDATE OF name, email ON users
FOR EACH ROW
EXECUTE FUNCTION update_order_user_info();
```

---

## 9.2 SQL avancé et ORM (Hibernate, Sequelize, Prisma)

### 9.2.1 SQL avancé

**Jointures:**

```sql
-- INNER JOIN: Seulement les correspondances
SELECT u.name, o.id AS order_id, o.total
FROM users u
INNER JOIN orders o ON u.id = o.user_id;

-- LEFT JOIN: Tous les users, même sans commandes
SELECT u.name, COUNT(o.id) AS order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id, u.name;

-- RIGHT JOIN: Toutes les commandes (équivalent inversé)
SELECT o.id, u.name
FROM users u
RIGHT JOIN orders o ON u.id = o.user_id;

-- FULL OUTER JOIN: Union complète
SELECT u.name, o.id
FROM users u
FULL OUTER JOIN orders o ON u.id = o.user_id;
```

**Sous-requêtes:**

```sql
-- Utilisateurs ayant commandé plus de 1000 CHF
SELECT name, email
FROM users
WHERE id IN (
    SELECT user_id
    FROM orders
    WHERE total > 1000
);

-- Produits jamais commandés
SELECT name, price
FROM products
WHERE id NOT IN (
    SELECT DISTINCT product_id
    FROM order_items
);

-- Total des commandes par utilisateur (sous-requête corrélée)
SELECT 
    u.name,
    (SELECT COUNT(*) FROM orders o WHERE o.user_id = u.id) AS order_count,
    (SELECT COALESCE(SUM(total), 0) FROM orders o WHERE o.user_id = u.id) AS total_spent
FROM users u;
```

**CTEs (Common Table Expressions):**

```sql
-- Calculer le total par catégorie
WITH category_sales AS (
    SELECT 
        p.category,
        SUM(oi.quantity * oi.unit_price) AS total_sales
    FROM products p
    JOIN order_items oi ON p.id = oi.product_id
    GROUP BY p.category
)
SELECT 
    category,
    total_sales,
    ROUND((total_sales / SUM(total_sales) OVER ()) * 100, 2) AS percentage
FROM category_sales
ORDER BY total_sales DESC;

-- CTE récursif: Hiérarchie de catégories
WITH RECURSIVE category_tree AS (
    -- Anchor: catégories racines
    SELECT id, name, parent_id, 0 AS level
    FROM categories
    WHERE parent_id IS NULL
    
    UNION ALL
    
    -- Récursion: sous-catégories
    SELECT c.id, c.name, c.parent_id, ct.level + 1
    FROM categories c
    JOIN category_tree ct ON c.parent_id = ct.id
)
SELECT * FROM category_tree ORDER BY level, name;
```

**Window Functions:**

```sql
-- Classement des produits par prix dans leur catégorie
SELECT 
    name,
    category,
    price,
    RANK() OVER (PARTITION BY category ORDER BY price DESC) AS price_rank
FROM products;

-- Running total des ventes par jour
SELECT 
    DATE(created_at) AS date,
    SUM(total) AS daily_total,
    SUM(SUM(total)) OVER (ORDER BY DATE(created_at)) AS running_total
FROM orders
GROUP BY DATE(created_at)
ORDER BY date;

-- Top 3 produits par catégorie
WITH ranked_products AS (
    SELECT 
        name,
        category,
        price,
        ROW_NUMBER() OVER (PARTITION BY category ORDER BY price DESC) AS rn
    FROM products
)
SELECT name, category, price
FROM ranked_products
WHERE rn <= 3;
```

**Transactions:**

```sql
BEGIN;

-- Décrémenter le stock
UPDATE products
SET stock = stock - 5
WHERE id = 101 AND stock >= 5;

-- Vérifier qu'une ligne a été affectée
IF NOT FOUND THEN
    ROLLBACK;
    RAISE EXCEPTION 'Insufficient stock';
END IF;

-- Créer la commande
INSERT INTO orders (user_id, total, status)
VALUES (1, 199.95, 'PENDING')
RETURNING id INTO order_id;

-- Ajouter les items
INSERT INTO order_items (order_id, product_id, quantity, unit_price)
VALUES (order_id, 101, 5, 39.99);

COMMIT;
```

### 9.2.2 ORM - Hibernate (Java)

```java
// Entity
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    @Column(nullable = false)
    private String name;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Order> orders = new ArrayList<>();
    
    @CreationTimestamp
    @Column(updatable = false)
    private LocalDateTime createdAt;
    
    @UpdateTimestamp
    private LocalDateTime updatedAt;
    
    // Getters, setters
}

@Entity
@Table(name = "orders")
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;
    
    @Column(nullable = false)
    private BigDecimal total;
    
    @Enumerated(EnumType.STRING)
    private OrderStatus status;
    
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<OrderItem> items = new ArrayList<>();
    
    @CreationTimestamp
    private LocalDateTime createdAt;
    
    // Méthodes helper
    public void addItem(OrderItem item) {
        items.add(item);
        item.setOrder(this);
    }
    
    public void removeItem(OrderItem item) {
        items.remove(item);
        item.setOrder(null);
    }
}

// Repository
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    
    @Query("SELECT u FROM User u LEFT JOIN FETCH u.orders WHERE u.id = :id")
    Optional<User> findByIdWithOrders(@Param("id") Long id);
    
    @Query("SELECT u FROM User u WHERE u.createdAt >= :since")
    List<User> findRecentUsers(@Param("since") LocalDateTime since);
}

// Service
@Service
@Transactional
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;
    
    @Autowired
    private ProductRepository productRepository;
    
    public Order createOrder(Long userId, List<OrderItemRequest> items) {
        User user = userRepository.findById(userId)
            .orElseThrow(() -> new UserNotFoundException());
        
        Order order = new Order();
        order.setUser(user);
        order.setStatus(OrderStatus.PENDING);
        
        BigDecimal total = BigDecimal.ZERO;
        
        for (OrderItemRequest itemReq : items) {
            Product product = productRepository.findById(itemReq.getProductId())
                .orElseThrow(() -> new ProductNotFoundException());
            
            // Vérifier le stock
            if (product.getStock() < itemReq.getQuantity()) {
                throw new InsufficientStockException();
            }
            
            // Créer l'item
            OrderItem item = new OrderItem();
            item.setProduct(product);
            item.setQuantity(itemReq.getQuantity());
            item.setUnitPrice(product.getPrice());
            order.addItem(item);
            
            // Décrémenter le stock
            product.setStock(product.getStock() - itemReq.getQuantity());
            
            // Ajouter au total
            total = total.add(product.getPrice().multiply(
                BigDecimal.valueOf(itemReq.getQuantity())
            ));
        }
        
        order.setTotal(total);
        
        return orderRepository.save(order);
    }
}
```

### 9.2.3 ORM - Sequelize (Node.js)

```javascript
// models/User.js
const { DataTypes } = require('sequelize');

module.exports = (sequelize) => {
  const User = sequelize.define('User', {
    id: {
      type: DataTypes.INTEGER,
      primaryKey: true,
      autoIncrement: true,
    },
    email: {
      type: DataTypes.STRING,
      allowNull: false,
      unique: true,
      validate: {
        isEmail: true,
      },
    },
    name: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    passwordHash: {
      type: DataTypes.STRING,
      allowNull: false,
    },
  }, {
    tableName: 'users',
    timestamps: true,
    underscored: true,
  });

  User.associate = (models) => {
    User.hasMany(models.Order, {
      foreignKey: 'userId',
      as: 'orders',
    });
  };

  return User;
};

// models/Order.js
module.exports = (sequelize) => {
  const Order = sequelize.define('Order', {
    id: {
      type: DataTypes.INTEGER,
      primaryKey: true,
      autoIncrement: true,
    },
    userId: {
      type: DataTypes.INTEGER,
      allowNull: false,
    },
    total: {
      type: DataTypes.DECIMAL(10, 2),
      allowNull: false,
    },
    status: {
      type: DataTypes.ENUM('PENDING', 'PAID', 'SHIPPED', 'DELIVERED', 'CANCELLED'),
      defaultValue: 'PENDING',
    },
  }, {
    tableName: 'orders',
    timestamps: true,
    underscored: true,
  });

  Order.associate = (models) => {
    Order.belongsTo(models.User, {
      foreignKey: 'userId',
      as: 'user',
    });
    
    Order.belongsToMany(models.Product, {
      through: models.OrderItem,
      foreignKey: 'orderId',
      as: 'products',
    });
  };

  return Order;
};

// services/orderService.js
const { sequelize, User, Order, Product, OrderItem } = require('../models');

async function createOrder(userId, items) {
  const transaction = await sequelize.transaction();

  try {
    // Vérifier l'utilisateur
    const user = await User.findByPk(userId, { transaction });
    if (!user) {
      throw new Error('User not found');
    }

    // Créer la commande
    const order = await Order.create({
      userId,
      total: 0,
      status: 'PENDING',
    }, { transaction });

    let total = 0;

    // Ajouter les items
    for (const item of items) {
      const product = await Product.findByPk(item.productId, {
        transaction,
        lock: transaction.LOCK.UPDATE,  // Lock pessimiste
      });

      if (!product) {
        throw new Error(`Product ${item.productId} not found`);
      }

      if (product.stock < item.quantity) {
        throw new Error(`Insufficient stock for ${product.name}`);
      }

      // Créer l'item
      await OrderItem.create({
        orderId: order.id,
        productId: product.id,
        quantity: item.quantity,
        unitPrice: product.price,
      }, { transaction });

      // Décrémenter le stock
      await product.update({
        stock: product.stock - item.quantity,
      }, { transaction });

      total += product.price * item.quantity;
    }

    // Mettre à jour le total
    await order.update({ total }, { transaction });

    await transaction.commit();

    // Charger l'ordre complet avec relations
    return await Order.findByPk(order.id, {
      include: [
        { model: User, as: 'user' },
        { model: Product, as: 'products', through: { attributes: ['quantity', 'unitPrice'] } },
      ],
    });

  } catch (error) {
    await transaction.rollback();
    throw error;
  }
}

module.exports = { createOrder };
```

### 9.2.4 ORM - Prisma (Node.js/TypeScript)

```prisma
// schema.prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id           Int       @id @default(autoincrement())
  email        String    @unique
  name         String
  passwordHash String    @map("password_hash")
  createdAt    DateTime  @default(now()) @map("created_at")
  updatedAt    DateTime  @updatedAt @map("updated_at")
  
  orders       Order[]

  @@map("users")
}

model Product {
  id          Int      @id @default(autoincrement())
  name        String
  description String?
  price       Decimal  @db.Decimal(10, 2)
  stock       Int      @default(0)
  category    String?
  imageUrl    String?  @map("image_url")
  createdAt   DateTime @default(now()) @map("created_at")
  updatedAt   DateTime @updatedAt @map("updated_at")
  
  orderItems  OrderItem[]

  @@map("products")
}

model Order {
  id        Int         @id @default(autoincrement())
  userId    Int         @map("user_id")
  total     Decimal     @db.Decimal(10, 2)
  status    OrderStatus @default(PENDING)
  createdAt DateTime    @default(now()) @map("created_at")
  updatedAt DateTime    @updatedAt @map("updated_at")
  
  user      User        @relation(fields: [userId], references: [id], onDelete: Cascade)
  items     OrderItem[]

  @@index([userId])
  @@index([status])
  @@map("orders")
}

model OrderItem {
  id        Int     @id @default(autoincrement())
  orderId   Int     @map("order_id")
  productId Int     @map("product_id")
  quantity  Int
  unitPrice Decimal @db.Decimal(10, 2) @map("unit_price")
  
  order     Order   @relation(fields: [orderId], references: [id], onDelete: Cascade)
  product   Product @relation(fields: [productId], references: [id], onDelete: Restrict)

  @@unique([orderId, productId])
  @@index([orderId])
  @@index([productId])
  @@map("order_items")
}

enum OrderStatus {
  PENDING
  PAID
  SHIPPED
  DELIVERED
  CANCELLED
}
```

```typescript
// services/orderService.ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

interface OrderItemInput {
  productId: number;
  quantity: number;
}

async function createOrder(userId: number, items: OrderItemInput[]) {
  return await prisma.$transaction(async (tx) => {
    // Vérifier l'utilisateur
    const user = await tx.user.findUnique({
      where: { id: userId },
    });

    if (!user) {
      throw new Error('User not found');
    }

    let total = 0;
    const orderItems = [];

    // Valider et préparer les items
    for (const item of items) {
      const product = await tx.product.findUnique({
        where: { id: item.productId },
      });

      if (!product) {
        throw new Error(`Product ${item.productId} not found`);
      }

      if (product.stock < item.quantity) {
        throw new Error(`Insufficient stock for ${product.name}`);
      }

      // Décrémenter le stock
      await tx.product.update({
        where: { id: product.id },
        data: { stock: { decrement: item.quantity } },
      });

      orderItems.push({
        productId: product.id,
        quantity: item.quantity,
        unitPrice: product.price,
      });

      total += Number(product.price) * item.quantity;
    }

    // Créer la commande avec les items
    const order = await tx.order.create({
      data: {
        userId,
        total,
        status: 'PENDING',
        items: {
          create: orderItems,
        },
      },
      include: {
        user: true,
        items: {
          include: {
            product: true,
          },
        },
      },
    });

    return order;
  });
}

// Requêtes avancées
async function getUserWithOrders(userId: number) {
  return await prisma.user.findUnique({
    where: { id: userId },
    include: {
      orders: {
        include: {
          items: {
            include: {
              product: true,
            },
          },
        },
        orderBy: {
          createdAt: 'desc',
        },
      },
    },
  });
}

// Agrégations
async function getOrderStatistics() {
  const stats = await prisma.order.groupBy({
    by: ['status'],
    _count: {
      id: true,
    },
    _sum: {
      total: true,
    },
    _avg: {
      total: true,
    },
  });

  return stats;
}

export { createOrder, getUserWithOrders, getOrderStatistics };
```

---

## 9.3 Bases NoSQL (MongoDB, Redis, Firebase, Cassandra)

### 9.3.1 MongoDB (Document Store)

```javascript
// Structure de document
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "email": "john@example.com",
  "name": "John Doe",
  "orders": [
    {
      "orderId": ObjectId("507f191e810c19729de860ea"),
      "date": ISODate("2024-10-20T10:30:00Z"),
      "total": 299.99,
      "status": "DELIVERED",
      "items": [
        {
          "productId": ObjectId("507f191e810c19729de860eb"),
          "name": "Laptop",
          "price": 1299.99,
          "quantity": 1
        },
        {
          "productId": ObjectId("507f191e810c19729de860ec"),
          "name": "Mouse",
          "price": 29.99,
          "quantity": 2
        }
      ]
    }
  ],
  "createdAt": ISODate("2024-01-15T08:00:00Z")
}

// Requêtes MongoDB
const { MongoClient } = require('mongodb');

const client = new MongoClient('mongodb://localhost:27017');
const db = client.db('ecommerce');

// Insertion
await db.collection('users').insertOne({
  email: 'jane@example.com',
  name: 'Jane Smith',
  orders: [],
  createdAt: new Date()
});

// Recherche
const user = await db.collection('users').findOne({
  email: 'john@example.com'
});

// Mise à jour (ajouter une commande)
await db.collection('users').updateOne(
  { email: 'john@example.com' },
  {
    $push: {
      orders: {
        orderId: new ObjectId(),
        date: new Date(),
        total: 159.99,
        status: 'PENDING',
        items: [
          { productId: new ObjectId(), name: 'Keyboard', price: 79.99, quantity: 2 }
        ]
      }
    }
  }
);

// Recherche avancée
const recentOrders = await db.collection('users').aggregate([
  { $unwind: '$orders' },
  { $match: { 'orders.date': { $gte: new Date('2024-10-01') } } },
  { $group: {
    _id: '$_id',
    email: { $first: '$email' },
    name: { $first: '$name' },
    totalOrders: { $sum: 1 },
    totalSpent: { $sum: '$orders.total' }
  }},
  { $sort: { totalSpent: -1 } },
  { $limit: 10 }
]).toArray();

// Index
await db.collection('users').createIndex({ email: 1 }, { unique: true });
await db.collection('users').createIndex({ 'orders.date': -1 });
```

**Quand utiliser MongoDB:**
- Documents avec structure variable
- Besoin de flexibilité du schéma
- Lectures/écritures rapides
- Évolutivité horizontale
- Données hiérarchiques ou imbriquées

### 9.3.2 Redis (Cache/Key-Value Store)

```javascript
const redis = require('redis');
const client = redis.createClient();

await client.connect();

// String (cache simple)
await client.set('user:1000', JSON.stringify({
  name: 'John Doe',
  email: 'john@example.com'
}), { EX: 3600 });  // Expiration 1 heure

const user = JSON.parse(await client.get('user:1000'));

// Hash (objet)
await client.hSet('user:1001', {
  name: 'Jane Smith',
  email: 'jane@example.com',
  age: '30'
});

const name = await client.hGet('user:1001', 'name');
const allFields = await client.hGetAll('user:1001');

// List (file, pile)
await client.lPush('queue:emails', 'email1@example.com');
await client.lPush('queue:emails', 'email2@example.com');

const email = await client.rPop('queue:emails');  // FIFO

// Set (ensemble unique)
await client.sAdd('user:1000:tags', 'vip');
await client.sAdd('user:1000:tags', 'premium');

const tags = await client.sMembers('user:1000:tags');
const isVip = await client.sIsMember('user:1000:tags', 'vip');

// Sorted Set (classement)
await client.zAdd('leaderboard', [
  { score: 100, value: 'player1' },
  { score: 250, value: 'player2' },
  { score: 150, value: 'player3' }
]);

const topPlayers = await client.zRevRange('leaderboard', 0, 9);  // Top 10

// Pub/Sub
const subscriber = client.duplicate();
await subscriber.connect();

await subscriber.subscribe('orders', (message) => {
  console.log('New order:', message);
});

await client.publish('orders', JSON.stringify({ orderId: 123 }));

// Pattern: Cache-Aside
async function getUser(userId) {
  const cacheKey = `user:${userId}`;
  
  // Essayer le cache
  let user = await client.get(cacheKey);
  
  if (user) {
    return JSON.parse(user);
  }
  
  // Cache miss - charger de la DB
  user = await db.users.findOne({ id: userId });
  
  // Mettre en cache
  await client.set(cacheKey, JSON.stringify(user), { EX: 3600 });
  
  return user;
}
```

**Quand utiliser Redis:**
- Cache haute performance
- Sessions utilisateur
- Rate limiting
- Leaderboards / Classements
- Pub/Sub temps réel
- Files d'attente

### 9.3.3 Comparaison SQL vs NoSQL

| Critère | SQL | NoSQL |
|---------|-----|-------|
| **Schéma** | Rigide, défini à l'avance | Flexible, évolutif |
| **Scalabilité** | Verticale (scale-up) | Horizontale (scale-out) |
| **Transactions** | ACID complet | BASE (eventual consistency) |
| **Relations** | Joins natifs | Embed ou refs manuelles |
| **Requêtes** | SQL puissant | API spécifiques |
| **Cas d'usage** | Données structurées, transactionnelles | Données non structurées, Big Data |

---

## 9.4 Gestion des migrations et intégrité des données

### 9.4.1 Migrations (Flyway/Liquibase pour Java, Sequelize/Prisma pour Node.js)

**Flyway (Java):**

```sql
-- V1__Initial_schema.sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL
);

-- V2__Add_orders_table.sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    total DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- V3__Add_status_to_orders.sql
ALTER TABLE orders 
ADD COLUMN status VARCHAR(50) DEFAULT 'PENDING';

-- V4__Create_index_orders_status.sql
CREATE INDEX idx_orders_status ON orders(status);
```

**Sequelize (Node.js):**

```javascript
// migrations/20241020100000-create-users.js
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('users', {
      id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true,
      },
      email: {
        type: Sequelize.STRING,
        allowNull: false,
        unique: true,
      },
      name: {
        type: Sequelize.STRING,
        allowNull: false,
      },
      created_at: {
        type: Sequelize.DATE,
        allowNull: false,
      },
      updated_at: {
        type: Sequelize.DATE,
        allowNull: false,
      },
    });

    await queryInterface.addIndex('users', ['email']);
  },

  down: async (queryInterface) => {
    await queryInterface.dropTable('users');
  },
};

// migrations/20241020110000-add-phone-to-users.js
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.addColumn('users', 'phone', {
      type: Sequelize.STRING,
      allowNull: true,
    });
  },

  down: async (queryInterface) => {
    await queryInterface.removeColumn('users', 'phone');
  },
};

// Exécution
// npx sequelize-cli db:migrate
// npx sequelize-cli db:migrate:undo (rollback)
```

**Prisma:**

```bash
# Créer une migration
npx prisma migrate dev --name add_phone_to_users

# Appliquer en production
npx prisma migrate deploy

# Réinitialiser (dev only)
npx prisma migrate reset
```

### 9.4.2 Intégrité des données

**Contraintes:**

```sql
-- NOT NULL
CREATE TABLE users (
    email VARCHAR(255) NOT NULL
);

-- UNIQUE
CREATE TABLE users (
    email VARCHAR(255) UNIQUE
);

-- CHECK
CREATE TABLE products (
    price DECIMAL(10, 2) CHECK (price >= 0),
    stock INTEGER CHECK (stock >= 0)
);

-- DEFAULT
CREATE TABLE orders (
    status VARCHAR(50) DEFAULT 'PENDING',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- FOREIGN KEY avec cascades
CREATE TABLE orders (
    user_id INTEGER REFERENCES users(id) 
        ON DELETE CASCADE 
        ON UPDATE CASCADE
);
```

**Triggers:**

```sql
-- Mettre à jour le stock lors d'une commande
CREATE OR REPLACE FUNCTION update_product_stock()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE products
    SET stock = stock - NEW.quantity
    WHERE id = NEW.product_id;
    
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER order_item_inserted
AFTER INSERT ON order_items
FOR EACH ROW
EXECUTE FUNCTION update_product_stock();

-- Audit log automatique
CREATE TABLE audit_log (
    id SERIAL PRIMARY KEY,
    table_name VARCHAR(50),
    operation VARCHAR(10),
    old_data JSONB,
    new_data JSONB,
    user_id INTEGER,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE OR REPLACE FUNCTION log_changes()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO audit_log (table_name, operation, old_data, new_data)
    VALUES (
        TG_TABLE_NAME,
        TG_OP,
        CASE WHEN TG_OP = 'DELETE' THEN row_to_json(OLD) ELSE NULL END,
        CASE WHEN TG_OP IN ('INSERT', 'UPDATE') THEN row_to_json(NEW) ELSE NULL END
    );
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER users_audit
AFTER INSERT OR UPDATE OR DELETE ON users
FOR EACH ROW
EXECUTE FUNCTION log_changes();
```

---

## 📝 Résumé du chapitre

✅ **Modélisation:** MCD/MLD, normalisation (1NF, 2NF, 3NF), dénormalisation stratégique

✅ **SQL avancé:** Joins, sous-requêtes, CTEs, window functions, transactions

✅ **ORM:** Hibernate, Sequelize, Prisma pour abstraction et productivité

✅ **NoSQL:** MongoDB (documents), Redis (cache), différents modèles de données

✅ **Migrations:** Gestion de schéma avec versioning et rollback

✅ **Intégrité:** Contraintes, foreign keys, triggers pour cohérence

---

## 💡 Exercices pratiques

### Exercice 1
Modéliser et créer une base pour un système de bibliothèque (livres, auteurs, emprunts).

### Exercice 2
Écrire des requêtes SQL avancées (CTEs, window functions) pour statistiques de ventes.

### Exercice 3
Implémenter un système de cache Redis devant une base PostgreSQL.

---

## 📚 Pour aller plus loin

- 📖 **"Database Design for Mere Mortals"** - Michael J. Hernandez
- 📖 **"MongoDB: The Definitive Guide"** - Kristina Chodorow
- 📖 **"Redis in Action"** - Josiah Carlson
- 🔗 **postgresqltutorial.com**: SQL et PostgreSQL

---

[⬅️ Chapitre 8](./chapitre-8-developpement-desktop-mobile.md) | [➡️ Partie IV](../partie-4-securite-performance-maintenance/README.md)

