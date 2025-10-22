# Chapitre 4 : Bonnes pratiques de conception

> **Objectif:** Maîtriser les paradigmes de programmation, les principes fondamentaux (SOLID, DRY, KISS, YAGNI), les design patterns et la gestion de la dette technique.

---

## 📋 Table des matières

1. [Paradigmes de programmation](#41-paradigmes-de-programmation-procédural-objet-fonctionnel)
2. [Principes SOLID, DRY, KISS, YAGNI](#42-principes-solid-dry-kiss-yagni)
3. [Patterns de conception](#43-patterns-de-conception-factory-singleton-observer-adapter)
4. [Gestion de la dette technique et refactoring](#44-gestion-de-la-dette-technique-et-refactoring)

---

## 4.1 Paradigmes de programmation (procédural, objet, fonctionnel)

### 4.1.1 Programmation procédurale

**Principe:** Organisation du code en procédures/fonctions manipulant des données.

```c
// Exemple en C
typedef struct {
    int id;
    char name[50];
    float balance;
} Account;

// Fonctions opérant sur les données
void deposit(Account* account, float amount) {
    account->balance += amount;
}

void withdraw(Account* account, float amount) {
    if (account->balance >= amount) {
        account->balance -= amount;
    }
}

float get_balance(Account* account) {
    return account->balance;
}

// Utilisation
Account account = {1, "John Doe", 1000.0};
deposit(&account, 500.0);
withdraw(&account, 200.0);
printf("Balance: %.2f\n", get_balance(&account));
```

**✅ Avantages:**
- Simple à comprendre
- Exécution directe et efficace
- Adapté aux scripts et petits programmes

**❌ Inconvénients:**
- Difficile à maintenir pour grands systèmes
- Données et comportements séparés
- Réutilisabilité limitée
- Couplage fort

### 4.1.2 Programmation orientée objet (POO)

**Principes fondamentaux:**

#### 1. Encapsulation

```java
public class BankAccount {
    // État privé (données cachées)
    private Long id;
    private String owner;
    private BigDecimal balance;
    
    // Interface publique (comportements exposés)
    public void deposit(BigDecimal amount) {
        validateAmount(amount);
        this.balance = this.balance.add(amount);
    }
    
    public void withdraw(BigDecimal amount) {
        validateAmount(amount);
        if (this.balance.compareTo(amount) < 0) {
            throw new InsufficientFundsException();
        }
        this.balance = this.balance.subtract(amount);
    }
    
    public BigDecimal getBalance() {
        return this.balance;  // Retour d'une copie, pas la référence
    }
    
    // Méthode privée (détail d'implémentation)
    private void validateAmount(BigDecimal amount) {
        if (amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
    }
}
```

#### 2. Héritage

```java
// Classe de base
public abstract class Account {
    protected String accountNumber;
    protected BigDecimal balance;
    
    public abstract void calculateInterest();
    
    public void deposit(BigDecimal amount) {
        balance = balance.add(amount);
    }
}

// Classes dérivées
public class SavingsAccount extends Account {
    private static final BigDecimal INTEREST_RATE = new BigDecimal("0.03");
    
    @Override
    public void calculateInterest() {
        BigDecimal interest = balance.multiply(INTEREST_RATE);
        deposit(interest);
    }
}

public class CheckingAccount extends Account {
    private BigDecimal overdraftLimit;
    
    @Override
    public void calculateInterest() {
        // Pas d'intérêts pour compte courant
    }
    
    @Override
    public void withdraw(BigDecimal amount) {
        if (balance.subtract(amount).compareTo(overdraftLimit.negate()) < 0) {
            throw new InsufficientFundsException();
        }
        balance = balance.subtract(amount);
    }
}
```

#### 3. Polymorphisme

```java
// Interface
public interface PaymentMethod {
    PaymentResult process(Payment payment);
    boolean supports(PaymentType type);
}

// Implémentations multiples
public class CreditCardPayment implements PaymentMethod {
    @Override
    public PaymentResult process(Payment payment) {
        // Logique carte de crédit
        return new PaymentResult(true, "CC-" + UUID.randomUUID());
    }
    
    @Override
    public boolean supports(PaymentType type) {
        return type == PaymentType.CREDIT_CARD;
    }
}

public class PayPalPayment implements PaymentMethod {
    @Override
    public PaymentResult process(Payment payment) {
        // Logique PayPal
        return new PaymentResult(true, "PP-" + UUID.randomUUID());
    }
    
    @Override
    public boolean supports(PaymentType type) {
        return type == PaymentType.PAYPAL;
    }
}

// Utilisation polymorphique
public class PaymentProcessor {
    private List<PaymentMethod> paymentMethods;
    
    public PaymentResult processPayment(Payment payment) {
        PaymentMethod method = paymentMethods.stream()
            .filter(m -> m.supports(payment.getType()))
            .findFirst()
            .orElseThrow(() -> new UnsupportedPaymentException());
        
        return method.process(payment);  // Polymorphisme !
    }
}
```

#### 4. Abstraction

```java
// Abstraction de haut niveau
public interface OrderService {
    Order createOrder(CreateOrderRequest request);
    Order getOrder(Long orderId);
    void cancelOrder(Long orderId);
}

// Implémentation concrète (détails cachés)
@Service
public class OrderServiceImpl implements OrderService {
    // Dépendances
    @Autowired private OrderRepository orderRepository;
    @Autowired private InventoryService inventoryService;
    @Autowired private PaymentService paymentService;
    @Autowired private NotificationService notificationService;
    
    @Override
    @Transactional
    public Order createOrder(CreateOrderRequest request) {
        // Orchestration complexe cachée derrière l'abstraction
        Order order = buildOrder(request);
        reserveInventory(order);
        processPayment(order);
        orderRepository.save(order);
        sendConfirmation(order);
        return order;
    }
    
    // Détails d'implémentation privés
    private Order buildOrder(CreateOrderRequest request) { /* ... */ }
    private void reserveInventory(Order order) { /* ... */ }
    private void processPayment(Order order) { /* ... */ }
    private void sendConfirmation(Order order) { /* ... */ }
}

// Utilisation simple grâce à l'abstraction
OrderService orderService = /* injection */;
Order order = orderService.createOrder(request);  // Simple !
```

### 4.1.3 Programmation fonctionnelle

**Principes:**
- Fonctions pures (pas d'effets de bord)
- Immuabilité
- Fonctions de première classe
- Composition de fonctions

**Exemple: Traitement de données**

```java
// Style impératif (procédural)
List<Order> getExpensiveOrders(List<Order> orders) {
    List<Order> result = new ArrayList<>();
    for (Order order : orders) {
        if (order.getTotal().compareTo(new BigDecimal("1000")) > 0) {
            result.add(order);
        }
    }
    return result;
}

// Style fonctionnel
List<Order> getExpensiveOrders(List<Order> orders) {
    return orders.stream()
        .filter(order -> order.getTotal().compareTo(new BigDecimal("1000")) > 0)
        .collect(Collectors.toList());
}

// Composition de fonctions
Function<Order, BigDecimal> calculateDiscount = order -> {
    BigDecimal total = order.getTotal();
    return total.multiply(new BigDecimal("0.10"));
};

Function<Order, BigDecimal> calculateTax = order -> {
    BigDecimal total = order.getTotal();
    return total.multiply(new BigDecimal("0.20"));
};

Function<Order, BigDecimal> calculateFinalPrice = 
    calculateDiscount.andThen(discountedPrice -> 
        originalPrice -> originalPrice.subtract(discountedPrice)
    ).andThen(calculateTax);

// Utilisation
BigDecimal finalPrice = orders.stream()
    .map(calculateFinalPrice)
    .reduce(BigDecimal.ZERO, BigDecimal::add);
```

**Immuabilité:**

```java
// ❌ Objet mutable
public class MutableOrder {
    private List<OrderItem> items;
    
    public void addItem(OrderItem item) {
        items.add(item);  // Modification de l'état !
    }
}

// ✅ Objet immutable
public final class ImmutableOrder {
    private final List<OrderItem> items;
    
    public ImmutableOrder(List<OrderItem> items) {
        this.items = List.copyOf(items);  // Copie défensive
    }
    
    public ImmutableOrder addItem(OrderItem item) {
        List<OrderItem> newItems = new ArrayList<>(this.items);
        newItems.add(item);
        return new ImmutableOrder(newItems);  // Nouvel objet !
    }
    
    public List<OrderItem> getItems() {
        return items;  // Déjà immutable
    }
}
```

**Fonctions de première classe:**

```javascript
// JavaScript - Fonctions comme valeurs
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const multiply = (a, b) => a * b;

// Fonction d'ordre supérieur
const calculate = (operation) => (a, b) => operation(a, b);

const adder = calculate(add);
console.log(adder(5, 3));  // 8

// Currying
const discount = (percentage) => (price) => price * (1 - percentage / 100);

const tenPercentOff = discount(10);
console.log(tenPercentOff(100));  // 90

const twentyPercentOff = discount(20);
console.log(twentyPercentOff(100));  // 80
```

---

## 4.2 Principes SOLID, DRY, KISS, YAGNI

### 4.2.1 SOLID

#### S - Single Responsibility Principle (SRP)

**Principe:** Une classe doit avoir une seule raison de changer.

```java
// ❌ Violation de SRP - Classe avec multiples responsabilités
public class User {
    private String email;
    private String password;
    
    // Responsabilité 1: Gestion des données
    public void setEmail(String email) { this.email = email; }
    
    // Responsabilité 2: Validation
    public boolean isValidEmail() {
        return email.matches("^[A-Za-z0-9+_.-]+@(.+)$");
    }
    
    // Responsabilité 3: Persistance
    public void save() {
        // Logique SQL
        Connection conn = DriverManager.getConnection(DB_URL);
        PreparedStatement stmt = conn.prepareStatement("INSERT INTO users...");
        stmt.executeUpdate();
    }
    
    // Responsabilité 4: Notification
    public void sendWelcomeEmail() {
        // Logique d'envoi email
    }
}

// ✅ Respect de SRP - Une responsabilité par classe
public class User {
    private String email;
    private String password;
    
    // Getters/Setters uniquement
}

public class EmailValidator {
    public boolean isValid(String email) {
        return email.matches("^[A-Za-z0-9+_.-]+@(.+)$");
    }
}

public class UserRepository {
    public void save(User user) {
        // Logique de persistance
    }
}

public class UserNotificationService {
    public void sendWelcomeEmail(User user) {
        // Logique de notification
    }
}
```

#### O - Open/Closed Principle (OCP)

**Principe:** Ouvert à l'extension, fermé à la modification.

```java
// ❌ Violation de OCP - Modification nécessaire pour chaque nouveau type
public class DiscountCalculator {
    public BigDecimal calculate(Order order, String customerType) {
        if (customerType.equals("REGULAR")) {
            return order.getTotal().multiply(new BigDecimal("0.05"));
        } else if (customerType.equals("VIP")) {
            return order.getTotal().multiply(new BigDecimal("0.10"));
        } else if (customerType.equals("GOLD")) {  // Modification !
            return order.getTotal().multiply(new BigDecimal("0.15"));
        }
        return BigDecimal.ZERO;
    }
}

// ✅ Respect de OCP - Extension sans modification
public interface DiscountStrategy {
    BigDecimal calculate(Order order);
}

public class RegularCustomerDiscount implements DiscountStrategy {
    @Override
    public BigDecimal calculate(Order order) {
        return order.getTotal().multiply(new BigDecimal("0.05"));
    }
}

public class VIPCustomerDiscount implements DiscountStrategy {
    @Override
    public BigDecimal calculate(Order order) {
        return order.getTotal().multiply(new BigDecimal("0.10"));
    }
}

// Nouvelle stratégie - Pas de modification du code existant !
public class GoldCustomerDiscount implements DiscountStrategy {
    @Override
    public BigDecimal calculate(Order order) {
        return order.getTotal().multiply(new BigDecimal("0.15"));
    }
}

public class DiscountCalculator {
    private Map<CustomerType, DiscountStrategy> strategies;
    
    public BigDecimal calculate(Order order, CustomerType type) {
        DiscountStrategy strategy = strategies.get(type);
        return strategy.calculate(order);
    }
}
```

#### L - Liskov Substitution Principle (LSP)

**Principe:** Les sous-types doivent être substituables à leurs types de base.

```java
// ❌ Violation de LSP
public class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int width) { this.width = width; }
    public void setHeight(int height) { this.height = height; }
    public int getArea() { return width * height; }
}

public class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;  // Comportement inattendu !
    }
    
    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height;  // Comportement inattendu !
    }
}

// Test qui échoue
Rectangle rect = new Square();
rect.setWidth(5);
rect.setHeight(4);
assert rect.getArea() == 20;  // FAIL! (16 attendu pour un carré)

// ✅ Respect de LSP - Modèle correct
public interface Shape {
    int getArea();
}

public class Rectangle implements Shape {
    private final int width;
    private final int height;
    
    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public int getArea() {
        return width * height;
    }
}

public class Square implements Shape {
    private final int side;
    
    public Square(int side) {
        this.side = side;
    }
    
    @Override
    public int getArea() {
        return side * side;
    }
}
```

#### I - Interface Segregation Principle (ISP)

**Principe:** Pas de dépendance forcée sur des méthodes inutilisées.

```java
// ❌ Violation de ISP - Interface trop large
public interface Worker {
    void work();
    void eat();
    void sleep();
}

public class Human implements Worker {
    @Override public void work() { /* OK */ }
    @Override public void eat() { /* OK */ }
    @Override public void sleep() { /* OK */ }
}

public class Robot implements Worker {
    @Override public void work() { /* OK */ }
    @Override public void eat() { /* Pas de sens ! */ }
    @Override public void sleep() { /* Pas de sens ! */ }
}

// ✅ Respect de ISP - Interfaces spécifiques
public interface Workable {
    void work();
}

public interface Eatable {
    void eat();
}

public interface Sleepable {
    void sleep();
}

public class Human implements Workable, Eatable, Sleepable {
    @Override public void work() { /* ... */ }
    @Override public void eat() { /* ... */ }
    @Override public void sleep() { /* ... */ }
}

public class Robot implements Workable {
    @Override public void work() { /* ... */ }
    // Pas forcé d'implémenter eat() et sleep()
}
```

#### D - Dependency Inversion Principle (DIP)

**Principe:** Dépendre des abstractions, pas des implémentations.

```java
// ❌ Violation de DIP - Dépendance sur implémentation concrète
public class OrderService {
    private MySQLOrderRepository repository;  // Couplage fort !
    
    public OrderService() {
        this.repository = new MySQLOrderRepository();  // Création directe
    }
    
    public void createOrder(Order order) {
        repository.save(order);
    }
}

// ✅ Respect de DIP - Dépendance sur abstraction
public interface OrderRepository {
    void save(Order order);
    Order findById(Long id);
}

public class MySQLOrderRepository implements OrderRepository {
    @Override
    public void save(Order order) { /* MySQL */ }
    @Override
    public Order findById(Long id) { /* MySQL */ }
}

public class MongoDBOrderRepository implements OrderRepository {
    @Override
    public void save(Order order) { /* MongoDB */ }
    @Override
    public Order findById(Long id) { /* MongoDB */ }
}

public class OrderService {
    private final OrderRepository repository;  // Dépend de l'abstraction
    
    // Injection de dépendance
    public OrderService(OrderRepository repository) {
        this.repository = repository;
    }
    
    public void createOrder(Order order) {
        repository.save(order);  // Fonctionne avec n'importe quelle impl.
    }
}

// Configuration (Spring)
@Configuration
public class AppConfig {
    @Bean
    public OrderRepository orderRepository() {
        return new MySQLOrderRepository();  // Choix ici, pas dans OrderService
    }
    
    @Bean
    public OrderService orderService(OrderRepository repository) {
        return new OrderService(repository);
    }
}
```

### 4.2.2 DRY (Don't Repeat Yourself)

**Principe:** Éviter la duplication de connaissance.

```java
// ❌ Violation de DRY
public class UserController {
    public Response createUser(UserRequest request) {
        if (request.getEmail() == null || request.getEmail().isEmpty()) {
            return Response.error("Email is required");
        }
        if (!request.getEmail().matches("^[A-Za-z0-9+_.-]+@(.+)$")) {
            return Response.error("Email format invalid");
        }
        if (request.getPassword() == null || request.getPassword().length() < 8) {
            return Response.error("Password must be at least 8 characters");
        }
        // ...
    }
    
    public Response updateEmail(UpdateEmailRequest request) {
        if (request.getEmail() == null || request.getEmail().isEmpty()) {  // DUPLICATION
            return Response.error("Email is required");
        }
        if (!request.getEmail().matches("^[A-Za-z0-9+_.-]+@(.+)$")) {  // DUPLICATION
            return Response.error("Email format invalid");
        }
        // ...
    }
}

// ✅ Respect de DRY - Extraction de la logique commune
public class EmailValidator {
    private static final Pattern EMAIL_PATTERN = 
        Pattern.compile("^[A-Za-z0-9+_.-]+@(.+)$");
    
    public ValidationResult validate(String email) {
        if (email == null || email.isEmpty()) {
            return ValidationResult.error("Email is required");
        }
        if (!EMAIL_PATTERN.matcher(email).matches()) {
            return ValidationResult.error("Email format invalid");
        }
        return ValidationResult.success();
    }
}

public class UserController {
    @Autowired
    private EmailValidator emailValidator;
    
    public Response createUser(UserRequest request) {
        ValidationResult emailValidation = emailValidator.validate(request.getEmail());
        if (!emailValidation.isValid()) {
            return Response.error(emailValidation.getMessage());
        }
        // ...
    }
    
    public Response updateEmail(UpdateEmailRequest request) {
        ValidationResult emailValidation = emailValidator.validate(request.getEmail());
        if (!emailValidation.isValid()) {
            return Response.error(emailValidation.getMessage());
        }
        // ...
    }
}
```

### 4.2.3 KISS (Keep It Simple, Stupid)

**Principe:** Préférer la simplicité à la complexité.

```java
// ❌ Complexité inutile
public class ComplexCalculator {
    public int calculate(int a, int b, String operation) {
        AbstractCalculationStrategyFactory factory = 
            CalculationStrategyFactoryProvider.getInstance()
                .getFactory(CalculationMode.STANDARD);
        
        CalculationStrategy strategy = factory.createStrategy(operation);
        CalculationContext context = new CalculationContext();
        context.setFirstOperand(a);
        context.setSecondOperand(b);
        
        CalculationResult result = strategy.execute(context);
        return result.getValue();
    }
}

// ✅ Simple et direct
public class SimpleCalculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    public int subtract(int a, int b) {
        return a - b;
    }
    
    public int multiply(int a, int b) {
        return a * b;
    }
}
```

### 4.2.4 YAGNI (You Aren't Gonna Need It)

**Principe:** N'implémentez que ce qui est nécessaire maintenant.

```java
// ❌ Sur-engineering - Fonctionnalités non nécessaires
public class User {
    private String email;
    private String password;
    
    // Pas encore nécessaire
    private List<Address> addresses;
    private List<PaymentMethod> paymentMethods;
    private UserPreferences preferences;
    private List<Order> orderHistory;
    private LoyaltyPoints loyaltyPoints;
    
    // Méthode pas encore utilisée
    public void calculateLoyaltyPoints() { /* ... */ }
    public void exportDataToXML() { /* ... */ }
    public void migrateToVersion2() { /* ... */ }
}

// ✅ Seulement ce qui est nécessaire
public class User {
    private String email;
    private String password;
    
    // Ajouter le reste quand vraiment nécessaire
}
```

---

## 4.3 Patterns de conception (Factory, Singleton, Observer, Adapter, etc.)

### 4.3.1 Patterns créationnels

#### Factory Method

**Problème:** Créer des objets sans spécifier leur classe exacte.

```java
// Sans Factory
public class OrderProcessor {
    public void process(Order order) {
        PaymentProcessor processor;
        if (order.getPaymentType() == PaymentType.CREDIT_CARD) {
            processor = new CreditCardProcessor();
        } else if (order.getPaymentType() == PaymentType.PAYPAL) {
            processor = new PayPalProcessor();
        } else if (order.getPaymentType() == PaymentType.BITCOIN) {
            processor = new BitcoinProcessor();
        }
        // ...
    }
}

// Avec Factory
public interface PaymentProcessor {
    PaymentResult process(Payment payment);
}

public class PaymentProcessorFactory {
    public PaymentProcessor create(PaymentType type) {
        return switch (type) {
            case CREDIT_CARD -> new CreditCardProcessor();
            case PAYPAL -> new PayPalProcessor();
            case BITCOIN -> new BitcoinProcessor();
            default -> throw new UnsupportedPaymentException(type);
        };
    }
}

// Utilisation
public class OrderProcessor {
    @Autowired
    private PaymentProcessorFactory factory;
    
    public void process(Order order) {
        PaymentProcessor processor = factory.create(order.getPaymentType());
        PaymentResult result = processor.process(order.getPayment());
        // ...
    }
}
```

#### Singleton

**Problème:** Garantir une seule instance d'une classe.

```java
// ❌ Implémentation basique (pas thread-safe)
public class DatabaseConnection {
    private static DatabaseConnection instance;
    
    private DatabaseConnection() {
        // Initialisation coûteuse
    }
    
    public static DatabaseConnection getInstance() {
        if (instance == null) {  // Problème en multi-thread !
            instance = new DatabaseConnection();
        }
        return instance;
    }
}

// ✅ Thread-safe avec double-checked locking
public class DatabaseConnection {
    private static volatile DatabaseConnection instance;
    
    private DatabaseConnection() {
        // Initialisation
    }
    
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            synchronized (DatabaseConnection.class) {
                if (instance == null) {
                    instance = new DatabaseConnection();
                }
            }
        }
        return instance;
    }
}

// ✅✅ Meilleure approche: Enum (thread-safe par défaut)
public enum DatabaseConnection {
    INSTANCE;
    
    private Connection connection;
    
    DatabaseConnection() {
        // Initialisation
        this.connection = createConnection();
    }
    
    public Connection getConnection() {
        return connection;
    }
}

// Utilisation
DatabaseConnection.INSTANCE.getConnection();
```

#### Builder

**Problème:** Construire des objets complexes étape par étape.

```java
// Sans Builder - Constructeur illisible
public class User {
    public User(String email, String password, String firstName, 
                String lastName, String phone, String address, 
                boolean emailVerified, boolean phoneVerified, 
                LocalDate birthDate, String country) {
        // ...
    }
}

// Appel difficile à lire
User user = new User("john@example.com", "pass123", "John", "Doe", 
                     "+41123456789", "Street 1", false, false, 
                     LocalDate.of(1990, 1, 1), "CH");

// Avec Builder
public class User {
    private final String email;
    private final String password;
    private final String firstName;
    private final String lastName;
    private final String phone;
    private final String address;
    private final boolean emailVerified;
    private final boolean phoneVerified;
    private final LocalDate birthDate;
    private final String country;
    
    private User(Builder builder) {
        this.email = builder.email;
        this.password = builder.password;
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.phone = builder.phone;
        this.address = builder.address;
        this.emailVerified = builder.emailVerified;
        this.phoneVerified = builder.phoneVerified;
        this.birthDate = builder.birthDate;
        this.country = builder.country;
    }
    
    public static class Builder {
        // Obligatoires
        private final String email;
        private final String password;
        
        // Optionnels avec valeurs par défaut
        private String firstName = "";
        private String lastName = "";
        private String phone = "";
        private String address = "";
        private boolean emailVerified = false;
        private boolean phoneVerified = false;
        private LocalDate birthDate = null;
        private String country = "CH";
        
        public Builder(String email, String password) {
            this.email = email;
            this.password = password;
        }
        
        public Builder firstName(String firstName) {
            this.firstName = firstName;
            return this;
        }
        
        public Builder lastName(String lastName) {
            this.lastName = lastName;
            return this;
        }
        
        public Builder phone(String phone) {
            this.phone = phone;
            return this;
        }
        
        public Builder address(String address) {
            this.address = address;
            return this;
        }
        
        public Builder emailVerified(boolean emailVerified) {
            this.emailVerified = emailVerified;
            return this;
        }
        
        public Builder birthDate(LocalDate birthDate) {
            this.birthDate = birthDate;
            return this;
        }
        
        public Builder country(String country) {
            this.country = country;
            return this;
        }
        
        public User build() {
            validate();
            return new User(this);
        }
        
        private void validate() {
            if (email == null || !email.contains("@")) {
                throw new IllegalArgumentException("Invalid email");
            }
            if (password == null || password.length() < 8) {
                throw new IllegalArgumentException("Password too short");
            }
        }
    }
}

// Utilisation - Beaucoup plus lisible !
User user = new User.Builder("john@example.com", "password123")
    .firstName("John")
    .lastName("Doe")
    .phone("+41123456789")
    .birthDate(LocalDate.of(1990, 1, 1))
    .build();
```

### 4.3.2 Patterns structurels

#### Adapter

**Problème:** Faire fonctionner ensemble des interfaces incompatibles.

```java
// Interface attendue par notre application
public interface PaymentGateway {
    PaymentResult processPayment(BigDecimal amount, String currency);
}

// Service externe avec interface différente
public class StripeAPI {
    public ChargeResponse charge(int amountInCents, String currencyCode, String token) {
        // Logique Stripe
    }
}

// Adapter pour rendre StripeAPI compatible
public class StripeAdapter implements PaymentGateway {
    private final StripeAPI stripeAPI;
    
    public StripeAdapter(StripeAPI stripeAPI) {
        this.stripeAPI = stripeAPI;
    }
    
    @Override
    public PaymentResult processPayment(BigDecimal amount, String currency) {
        // Conversion des paramètres
        int amountInCents = amount.multiply(new BigDecimal("100")).intValue();
        String token = generateToken();
        
        // Appel de l'API externe
        ChargeResponse response = stripeAPI.charge(amountInCents, currency, token);
        
        // Conversion du résultat
        return new PaymentResult(
            response.isSuccessful(),
            response.getTransactionId(),
            response.getMessage()
        );
    }
}

// Utilisation uniforme
PaymentGateway gateway = new StripeAdapter(new StripeAPI());
PaymentResult result = gateway.processPayment(new BigDecimal("100.00"), "CHF");
```

#### Decorator

**Problème:** Ajouter des responsabilités dynamiquement à un objet.

```java
// Interface de base
public interface Coffee {
    String getDescription();
    BigDecimal getCost();
}

// Implémentation de base
public class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple coffee";
    }
    
    @Override
    public BigDecimal getCost() {
        return new BigDecimal("2.00");
    }
}

// Decorateur abstrait
public abstract class CoffeeDecorator implements Coffee {
    protected final Coffee decoratedCoffee;
    
    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }
    
    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription();
    }
    
    @Override
    public BigDecimal getCost() {
        return decoratedCoffee.getCost();
    }
}

// Decorateurs concrets
public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", milk";
    }
    
    @Override
    public BigDecimal getCost() {
        return decoratedCoffee.getCost().add(new BigDecimal("0.50"));
    }
}

public class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", sugar";
    }
    
    @Override
    public BigDecimal getCost() {
        return decoratedCoffee.getCost().add(new BigDecimal("0.20"));
    }
}

// Utilisation - Composition dynamique
Coffee coffee = new SimpleCoffee();
coffee = new MilkDecorator(coffee);
coffee = new SugarDecorator(coffee);

System.out.println(coffee.getDescription());  // "Simple coffee, milk, sugar"
System.out.println(coffee.getCost());  // 2.70
```

### 4.3.3 Patterns comportementaux

#### Observer

**Problème:** Notifier automatiquement des objets dépendants lors d'un changement d'état.

```java
// Subject (Observable)
public interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers();
}

// Observer
public interface Observer {
    void update(Order order);
}

// Subject concret
public class Order implements Subject {
    private Long id;
    private OrderStatus status;
    private List<Observer> observers = new ArrayList<>();
    
    @Override
    public void attach(Observer observer) {
        observers.add(observer);
    }
    
    @Override
    public void detach(Observer observer) {
        observers.remove(observer);
    }
    
    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(this);
        }
    }
    
    public void setStatus(OrderStatus status) {
        this.status = status;
        notifyObservers();  // Notification automatique
    }
    
    public OrderStatus getStatus() {
        return status;
    }
}

// Observers concrets
public class EmailNotificationObserver implements Observer {
    @Override
    public void update(Order order) {
        System.out.println("Sending email: Order " + order.getId() + 
                         " status changed to " + order.getStatus());
    }
}

public class SMSNotificationObserver implements Observer {
    @Override
    public void update(Order order) {
        System.out.println("Sending SMS: Order " + order.getId() + 
                         " status changed to " + order.getStatus());
    }
}

public class InventoryObserver implements Observer {
    @Override
    public void update(Order order) {
        if (order.getStatus() == OrderStatus.PAID) {
            System.out.println("Reserving inventory for order " + order.getId());
        }
    }
}

// Utilisation
Order order = new Order();
order.attach(new EmailNotificationObserver());
order.attach(new SMSNotificationObserver());
order.attach(new InventoryObserver());

order.setStatus(OrderStatus.PAID);  // Tous les observers sont notifiés !
```

#### Strategy

**Problème:** Définir une famille d'algorithmes interchangeables.

```java
// Strategy interface
public interface ShippingStrategy {
    BigDecimal calculateCost(Order order);
    int estimateDeliveryDays();
}

// Strategies concrètes
public class StandardShipping implements ShippingStrategy {
    @Override
    public BigDecimal calculateCost(Order order) {
        return new BigDecimal("5.00");
    }
    
    @Override
    public int estimateDeliveryDays() {
        return 5;
    }
}

public class ExpressShipping implements ShippingStrategy {
    @Override
    public BigDecimal calculateCost(Order order) {
        return order.getTotal().multiply(new BigDecimal("0.10"));
    }
    
    @Override
    public int estimateDeliveryDays() {
        return 2;
    }
}

public class PriorityShipping implements ShippingStrategy {
    @Override
    public BigDecimal calculateCost(Order order) {
        return new BigDecimal("20.00");
    }
    
    @Override
    public int estimateDeliveryDays() {
        return 1;
    }
}

// Context
public class ShippingCalculator {
    private ShippingStrategy strategy;
    
    public void setStrategy(ShippingStrategy strategy) {
        this.strategy = strategy;
    }
    
    public BigDecimal calculate(Order order) {
        return strategy.calculateCost(order);
    }
    
    public int estimateDelivery() {
        return strategy.estimateDeliveryDays();
    }
}

// Utilisation - Changement dynamique de comportement
ShippingCalculator calculator = new ShippingCalculator();

calculator.setStrategy(new StandardShipping());
System.out.println("Standard: " + calculator.calculate(order));

calculator.setStrategy(new ExpressShipping());
System.out.println("Express: " + calculator.calculate(order));

calculator.setStrategy(new PriorityShipping());
System.out.println("Priority: " + calculator.calculate(order));
```

---

## 4.4 Gestion de la dette technique et refactoring

### 4.4.1 Qu'est-ce que la dette technique?

**Définition:**
> La dette technique représente le coût futur du travail supplémentaire causé par le choix d'une solution rapide maintenant plutôt qu'une meilleure approche qui prendrait plus de temps.

**Types de dette technique:**

1. **Dette délibérée et prudente**
   - Décision consciente pour gagner du temps
   - Documentée et planifiée pour remboursement
   - Exemple: "On utilise une solution simple maintenant, on optimisera plus tard"

2. **Dette délibérée et imprudente**
   - Raccourcis sans considération des conséquences
   - Exemple: "Pas le temps de faire propre, on livrera comme ça"

3. **Dette accidentelle et prudente**
   - Apprentissage progressif
   - Exemple: "Maintenant qu'on comprend mieux, on devrait refaire autrement"

4. **Dette accidentelle et imprudente**
   - Manque de compétences ou de rigueur
   - Exemple: "On ne savait pas qu'il fallait utiliser des design patterns"

### 4.4.2 Indicateurs de dette technique

**Code smells (mauvaises odeurs):**

```java
// 1. Méthode trop longue (> 20 lignes)
public void processOrder(Order order) {
    // 100 lignes de code...
    // Difficile à comprendre et tester
}

// 2. Classe trop large (God Class)
public class OrderManager {
    // Gère commandes, paiements, inventaire, notifications...
    // 500 lignes de code, 30 méthodes
}

// 3. Duplication de code
public void calculateDiscountForRegular() {
    if (total > 100) {
        discount = total * 0.10;
    }
}

public void calculateDiscountForVIP() {
    if (total > 100) {  // DUPLICATION
        discount = total * 0.15;
    }
}

// 4. Commentaires excessifs (code pas clair)
// Get the user
User u = getU();
// Check if user is not null
if (u != null) {
    // Set the flag to true
    f = true;
}

// 5. Nommage obscur
int d;  // elapsed time in days
List<int[]> list1 = new ArrayList<>();

// 6. Paramètres nombreux (> 3)
public void createOrder(String name, String email, String phone, 
                       String address, String city, String zip, 
                       String country, String payment, String shipping) {
    // ...
}

// 7. Couplage fort
public class OrderService {
    private MySQLDatabase db = new MySQLDatabase();  // Dépendance directe
}
```

**Métriques:**
- **Cyclomatic Complexity:** Nombre de chemins indépendants (> 10 = problématique)
- **Code Coverage:** % de code couvert par les tests (< 70% = risqué)
- **Technical Debt Ratio:** Temps pour corriger / Temps de développement
- **Code Duplication:** % de code dupliqué (> 5% = problématique)

### 4.4.3 Techniques de refactoring

#### Extract Method

```java
// Avant
public void printInvoice(Order order) {
    System.out.println("***********************");
    System.out.println("       INVOICE         ");
    System.out.println("***********************");
    System.out.println("Order ID: " + order.getId());
    System.out.println("Customer: " + order.getCustomer().getName());
    System.out.println("Items:");
    for (OrderItem item : order.getItems()) {
        System.out.println("  - " + item.getProduct().getName() + 
                         ": " + item.getQuantity() + " x " + item.getPrice());
    }
    System.out.println("-----------------------");
    System.out.println("Total: " + order.getTotal());
}

// Après
public void printInvoice(Order order) {
    printHeader();
    printCustomerInfo(order);
    printItems(order);
    printTotal(order);
}

private void printHeader() {
    System.out.println("***********************");
    System.out.println("       INVOICE         ");
    System.out.println("***********************");
}

private void printCustomerInfo(Order order) {
    System.out.println("Order ID: " + order.getId());
    System.out.println("Customer: " + order.getCustomer().getName());
}

private void printItems(Order order) {
    System.out.println("Items:");
    for (OrderItem item : order.getItems()) {
        printItem(item);
    }
}

private void printItem(OrderItem item) {
    System.out.println("  - " + item.getProduct().getName() + 
                     ": " + item.getQuantity() + " x " + item.getPrice());
}

private void printTotal(Order order) {
    System.out.println("-----------------------");
    System.out.println("Total: " + order.getTotal());
}
```

#### Replace Conditional with Polymorphism

```java
// Avant
public double calculateShipping(Order order, String type) {
    if (type.equals("standard")) {
        return 5.0;
    } else if (type.equals("express")) {
        return order.getTotal() * 0.10;
    } else if (type.equals("priority")) {
        return 20.0;
    }
    return 0.0;
}

// Après (vu précédemment avec Strategy pattern)
```

#### Introduce Parameter Object

```java
// Avant
public Order createOrder(String customerName, String customerEmail, 
                        String customerPhone, String shippingAddress, 
                        String billingAddress) {
    // ...
}

// Après
public class CustomerInfo {
    private String name;
    private String email;
    private String phone;
    
    // getters/setters
}

public class AddressInfo {
    private String shippingAddress;
    private String billingAddress;
    
    // getters/setters
}

public Order createOrder(CustomerInfo customer, AddressInfo address) {
    // Plus lisible !
}
```

### 4.4.4 Stratégies de gestion

**Principe du Boy Scout:**
> "Laissez le code plus propre que vous ne l'avez trouvé"

```java
// Avant modification
public void addItem(Item item) {
    items.add(item);
    total = total + item.price;  // Variable exposée, calcul manuel
}

// Après amélioration (lors de la modification)
public void addItem(Item item) {
    items.add(item);
    recalculateTotal();  // Méthode dédiée
}

private void recalculateTotal() {
    this.total = items.stream()
        .map(Item::getPrice)
        .reduce(BigDecimal.ZERO, BigDecimal::add);
}
```

**Refactoring continu vs. Big Bang:**

```
✅ Approche continue (recommandée):
- Petites améliorations régulières
- Chaque commit améliore légèrement le code
- Tests passent toujours
- Risque faible

❌ Big Bang (risqué):
- Grande refonte d'un coup
- Branch de longue durée
- Tests cassés pendant longtemps
- Risque élevé de régression
```

---

## 📝 Résumé du chapitre

✅ **Paradigmes:** Procédural (fonctions), OOP (objets), Fonctionnel (immuabilité)

✅ **SOLID:** 5 principes pour code maintenable (SRP, OCP, LSP, ISP, DIP)

✅ **DRY, KISS, YAGNI:** Simplicité et pragmatisme

✅ **Design Patterns:** Solutions éprouvées aux problèmes récurrents

✅ **Dette technique:** Coût des compromis à court terme, doit être gérée

✅ **Refactoring:** Amélioration continue du code sans changer son comportement

---

## 💡 Exercices pratiques

### Exercice 1: Identifier les violations SOLID

Analysez ce code et identifiez les violations des principes SOLID:

```java
public class UserService {
    public void registerUser(String email, String password) {
        // Validation
        if (!email.contains("@")) {
            throw new Exception("Invalid email");
        }
        
        // Save to database
        Connection conn = DriverManager.getConnection(DB_URL);
        PreparedStatement stmt = conn.prepareStatement("INSERT INTO users...");
        stmt.executeUpdate();
        
        // Send email
        SMTPConnection smtp = new SMTPConnection();
        smtp.send(email, "Welcome!");
    }
}
```

### Exercice 2: Appliquer un design pattern

Refactorisez ce code en utilisant le pattern Strategy:

```java
public class PaymentProcessor {
    public void process(Payment payment) {
        if (payment.getType().equals("CARD")) {
            // Process card
        } else if (payment.getType().equals("PAYPAL")) {
            // Process PayPal
        } else if (payment.getType().equals("CRYPTO")) {
            // Process crypto
        }
    }
}
```

### Exercice 3: Refactoring

Refactorisez cette méthode trop longue en extrayant des méthodes plus petites.

---

## 📚 Pour aller plus loin

- 📖 **"Clean Code"** - Robert C. Martin
- 📖 **"Refactoring"** - Martin Fowler
- 📖 **"Design Patterns"** - Gang of Four (GoF)
- 📖 **"Head First Design Patterns"** - Freeman & Robson

---

[⬅️ Partie II](./README.md) | [➡️ Chapitre 5](./chapitre-5-developpement-collaboratif.md)

