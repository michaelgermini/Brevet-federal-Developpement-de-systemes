# Chapitre 6 : Développement orienté qualité

> **Objectif:** Maîtriser les différents types de tests, les stratégies TDD et BDD, les outils de test et la mesure de la qualité logicielle.

---

## 📋 Table des matières

1. [Tests unitaires, d'intégration et fonctionnels](#61-tests-unitaires-dintégration-et-fonctionnels)
2. [Stratégies de test : TDD, BDD](#62-stratégies-de-test-tdd-bdd)
3. [Outils de test](#63-outils-de-test-junit-pytest-jest-postman)
4. [Mesure de la couverture et validation logicielle](#64-mesure-de-la-couverture-et-validation-logicielle)

---

## 6.1 Tests unitaires, d'intégration et fonctionnels

### 6.1.1 Pyramide des tests

```
                    ┌─────────┐
                    │   E2E   │  ← Peu nombreux, lents, coûteux
                    │  Tests  │     Testent le système complet
                    └─────────┘
                  ┌─────────────┐
                  │ Integration │  ← Moyennement nombreux
                  │    Tests    │     Testent les interactions
                  └─────────────┘
              ┌───────────────────┐
              │   Unit Tests      │  ← Très nombreux, rapides
              │                   │     Testent composants isolés
              └───────────────────┘
              
70% Unit Tests
20% Integration Tests
10% E2E Tests
```

### 6.1.2 Tests unitaires

**Caractéristiques:**
- Testent une unité de code isolée (fonction, méthode, classe)
- Rapides à exécuter (< 1ms par test)
- Pas de dépendances externes (DB, réseau, fichiers)
- Utilisent des mocks/stubs pour les dépendances

**Exemple JUnit 5 (Java):**

```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

class OrderServiceTest {
    
    private OrderService orderService;
    private OrderRepository orderRepository;
    private PaymentService paymentService;
    
    @BeforeEach
    void setUp() {
        // Mocks des dépendances
        orderRepository = mock(OrderRepository.class);
        paymentService = mock(PaymentService.class);
        orderService = new OrderService(orderRepository, paymentService);
    }
    
    @Test
    @DisplayName("Devrait créer une commande valide")
    void shouldCreateValidOrder() {
        // Arrange (Préparation)
        CreateOrderRequest request = new CreateOrderRequest();
        request.setUserId(1L);
        request.addItem(new OrderItem(101L, 2));
        
        Order savedOrder = new Order(1L, request.getUserId());
        when(orderRepository.save(any(Order.class))).thenReturn(savedOrder);
        
        // Act (Action)
        Order result = orderService.createOrder(request);
        
        // Assert (Vérification)
        assertNotNull(result);
        assertEquals(1L, result.getId());
        assertEquals(1L, result.getUserId());
        verify(orderRepository, times(1)).save(any(Order.class));
    }
    
    @Test
    @DisplayName("Devrait lancer une exception si la commande est vide")
    void shouldThrowExceptionWhenOrderIsEmpty() {
        // Arrange
        CreateOrderRequest request = new CreateOrderRequest();
        request.setUserId(1L);
        // Pas d'items
        
        // Act & Assert
        assertThrows(InvalidOrderException.class, () -> {
            orderService.createOrder(request);
        });
        
        // Vérifier qu'on n'a pas appelé le repository
        verify(orderRepository, never()).save(any(Order.class));
    }
    
    @Test
    @DisplayName("Devrait calculer le total correctement")
    void shouldCalculateTotalCorrectly() {
        // Arrange
        Order order = new Order();
        order.addItem(new OrderItem(101L, "Product A", new BigDecimal("10.00"), 2));
        order.addItem(new OrderItem(102L, "Product B", new BigDecimal("15.50"), 3));
        
        // Act
        BigDecimal total = order.calculateTotal();
        
        // Assert
        assertEquals(new BigDecimal("66.50"), total);  // (10*2) + (15.50*3)
    }
    
    @ParameterizedTest
    @CsvSource({
        "0, false",
        "-10, false",
        "10, true",
        "100.50, true"
    })
    @DisplayName("Devrait valider les montants")
    void shouldValidateAmounts(BigDecimal amount, boolean expected) {
        // Act
        boolean result = orderService.isValidAmount(amount);
        
        // Assert
        assertEquals(expected, result);
    }
    
    @Test
    @Timeout(value = 100, unit = TimeUnit.MILLISECONDS)
    @DisplayName("Devrait s'exécuter rapidement")
    void shouldExecuteQuickly() {
        // Test de performance simple
        orderService.quickOperation();
    }
}
```

**Exemple Jest (JavaScript/TypeScript):**

```javascript
// orderService.test.js
describe('OrderService', () => {
    let orderService;
    let mockOrderRepository;
    let mockPaymentService;
    
    beforeEach(() => {
        // Créer des mocks
        mockOrderRepository = {
            save: jest.fn(),
            findById: jest.fn()
        };
        
        mockPaymentService = {
            processPayment: jest.fn()
        };
        
        orderService = new OrderService(mockOrderRepository, mockPaymentService);
    });
    
    afterEach(() => {
        jest.clearAllMocks();
    });
    
    describe('createOrder', () => {
        it('devrait créer une commande valide', async () => {
            // Arrange
            const request = {
                userId: 1,
                items: [
                    { productId: 101, quantity: 2 }
                ]
            };
            
            const savedOrder = { id: 1, userId: 1, items: request.items };
            mockOrderRepository.save.mockResolvedValue(savedOrder);
            
            // Act
            const result = await orderService.createOrder(request);
            
            // Assert
            expect(result).toEqual(savedOrder);
            expect(mockOrderRepository.save).toHaveBeenCalledTimes(1);
            expect(mockOrderRepository.save).toHaveBeenCalledWith(
                expect.objectContaining({
                    userId: 1,
                    items: request.items
                })
            );
        });
        
        it('devrait rejeter une commande vide', async () => {
            // Arrange
            const request = { userId: 1, items: [] };
            
            // Act & Assert
            await expect(orderService.createOrder(request))
                .rejects
                .toThrow('Order must contain at least one item');
            
            expect(mockOrderRepository.save).not.toHaveBeenCalled();
        });
        
        it('devrait calculer le total correctement', () => {
            // Arrange
            const order = {
                items: [
                    { price: 10.00, quantity: 2 },
                    { price: 15.50, quantity: 3 }
                ]
            };
            
            // Act
            const total = orderService.calculateTotal(order);
            
            // Assert
            expect(total).toBe(66.50);
        });
    });
    
    describe('validateOrder', () => {
        it.each([
            [null, false],
            [undefined, false],
            [{}, false],
            [{ items: [] }, false],
            [{ items: [{ productId: 1 }] }, true]
        ])('devrait valider correctement: %p → %p', (order, expected) => {
            expect(orderService.isValid(order)).toBe(expected);
        });
    });
});
```

**Exemple PyTest (Python):**

```python
# test_order_service.py
import pytest
from unittest.mock import Mock, patch
from decimal import Decimal

from services.order_service import OrderService
from models.order import Order
from exceptions import InvalidOrderException

@pytest.fixture
def order_repository():
    """Fixture pour mocker le repository."""
    return Mock()

@pytest.fixture
def payment_service():
    """Fixture pour mocker le service de paiement."""
    return Mock()

@pytest.fixture
def order_service(order_repository, payment_service):
    """Fixture pour créer le service avec les mocks."""
    return OrderService(order_repository, payment_service)

class TestOrderService:
    """Tests pour OrderService."""
    
    def test_create_valid_order(self, order_service, order_repository):
        """Devrait créer une commande valide."""
        # Arrange
        request = {
            'user_id': 1,
            'items': [
                {'product_id': 101, 'quantity': 2}
            ]
        }
        
        saved_order = Order(id=1, user_id=1)
        order_repository.save.return_value = saved_order
        
        # Act
        result = order_service.create_order(request)
        
        # Assert
        assert result.id == 1
        assert result.user_id == 1
        order_repository.save.assert_called_once()
    
    def test_empty_order_raises_exception(self, order_service, order_repository):
        """Devrait rejeter une commande vide."""
        # Arrange
        request = {'user_id': 1, 'items': []}
        
        # Act & Assert
        with pytest.raises(InvalidOrderException, match="at least one item"):
            order_service.create_order(request)
        
        order_repository.save.assert_not_called()
    
    @pytest.mark.parametrize("items,expected_total", [
        ([{'price': Decimal('10.00'), 'quantity': 2}], Decimal('20.00')),
        ([
            {'price': Decimal('10.00'), 'quantity': 2},
            {'price': Decimal('15.50'), 'quantity': 3}
        ], Decimal('66.50')),
        ([], Decimal('0.00'))
    ])
    def test_calculate_total(self, order_service, items, expected_total):
        """Devrait calculer le total correctement."""
        # Arrange
        order = Order(items=items)
        
        # Act
        total = order_service.calculate_total(order)
        
        # Assert
        assert total == expected_total
```

### 6.1.3 Tests d'intégration

**Caractéristiques:**
- Testent l'interaction entre plusieurs composants
- Peuvent utiliser des dépendances réelles (DB de test, etc.)
- Plus lents que les tests unitaires
- Détectent les problèmes d'intégration

**Exemple Spring Boot (Java):**

```java
@SpringBootTest
@AutoConfigureTestDatabase
@Transactional
class OrderIntegrationTest {
    
    @Autowired
    private OrderService orderService;
    
    @Autowired
    private OrderRepository orderRepository;
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private TestEntityManager entityManager;
    
    @Test
    @DisplayName("Intégration: Créer commande avec DB réelle")
    void shouldCreateOrderWithRealDatabase() {
        // Arrange - Créer des données de test
        User user = new User("john@example.com", "password");
        userRepository.save(user);
        
        Product product = new Product("Laptop", new BigDecimal("1299.99"));
        entityManager.persist(product);
        entityManager.flush();
        
        CreateOrderRequest request = new CreateOrderRequest();
        request.setUserId(user.getId());
        request.addItem(new OrderItemRequest(product.getId(), 1));
        
        // Act
        Order order = orderService.createOrder(request);
        
        // Assert
        assertNotNull(order.getId());
        
        // Vérifier en DB
        Order savedOrder = orderRepository.findById(order.getId()).orElseThrow();
        assertEquals(user.getId(), savedOrder.getUserId());
        assertEquals(1, savedOrder.getItems().size());
        assertEquals(new BigDecimal("1299.99"), savedOrder.getTotal());
    }
    
    @Test
    @DisplayName("Intégration: Transaction rollback sur erreur")
    void shouldRollbackTransactionOnError() {
        // Arrange
        User user = userRepository.save(new User("test@example.com", "pass"));
        
        CreateOrderRequest request = new CreateOrderRequest();
        request.setUserId(user.getId());
        // Produit inexistant -> erreur
        request.addItem(new OrderItemRequest(9999L, 1));
        
        // Act & Assert
        assertThrows(ProductNotFoundException.class, () -> {
            orderService.createOrder(request);
        });
        
        // Vérifier qu'aucune commande n'a été créée (rollback)
        assertEquals(0, orderRepository.count());
    }
}
```

**Exemple avec Testcontainers (DB réelle dans Docker):**

```java
@SpringBootTest
@Testcontainers
class OrderIntegrationWithContainersTest {
    
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:14")
        .withDatabaseName("test_db")
        .withUsername("test")
        .withPassword("test");
    
    @Container
    static GenericContainer<?> redis = new GenericContainer<>("redis:7")
        .withExposedPorts(6379);
    
    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
        registry.add("spring.redis.host", redis::getHost);
        registry.add("spring.redis.port", () -> redis.getMappedPort(6379));
    }
    
    @Autowired
    private OrderService orderService;
    
    @Test
    void shouldWorkWithRealDatabaseAndCache() {
        // Test avec PostgreSQL et Redis réels dans des containers
    }
}
```

### 6.1.4 Tests End-to-End (E2E)

**Caractéristiques:**
- Testent le système complet du point de vue utilisateur
- Incluent l'interface utilisateur, l'API, la base de données
- Les plus lents et fragiles
- Détectent les problèmes de bout en bout

**Exemple Cypress (Tests E2E web):**

```javascript
// cypress/e2e/order-flow.cy.js
describe('Flux de commande complet', () => {
    beforeEach(() => {
        // Réinitialiser la DB de test
        cy.task('db:seed');
        // Visiter la page
        cy.visit('/');
    });
    
    it('devrait permettre à un utilisateur de passer une commande', () => {
        // 1. Connexion
        cy.get('[data-testid="login-button"]').click();
        cy.get('[data-testid="email-input"]').type('user@example.com');
        cy.get('[data-testid="password-input"]').type('password123');
        cy.get('[data-testid="submit-button"]').click();
        
        // Vérifier la connexion réussie
        cy.url().should('include', '/dashboard');
        cy.get('[data-testid="welcome-message"]').should('contain', 'Welcome');
        
        // 2. Rechercher un produit
        cy.get('[data-testid="search-input"]').type('laptop');
        cy.get('[data-testid="search-button"]').click();
        
        // Vérifier les résultats
        cy.get('[data-testid="product-card"]').should('have.length.at.least', 1);
        
        // 3. Ajouter au panier
        cy.get('[data-testid="product-card"]').first().within(() => {
            cy.get('[data-testid="product-name"]').should('contain', 'Laptop');
            cy.get('[data-testid="add-to-cart"]').click();
        });
        
        // Vérifier la notification
        cy.get('[data-testid="notification"]').should('contain', 'Added to cart');
        cy.get('[data-testid="cart-badge"]').should('contain', '1');
        
        // 4. Aller au panier
        cy.get('[data-testid="cart-icon"]').click();
        cy.url().should('include', '/cart');
        
        // Vérifier le contenu
        cy.get('[data-testid="cart-item"]').should('have.length', 1);
        cy.get('[data-testid="cart-total"]').should('exist');
        
        // 5. Procéder au paiement
        cy.get('[data-testid="checkout-button"]').click();
        cy.url().should('include', '/checkout');
        
        // Remplir les informations de paiement
        cy.get('[data-testid="card-number"]').type('4242424242424242');
        cy.get('[data-testid="card-expiry"]').type('12/25');
        cy.get('[data-testid="card-cvc"]').type('123');
        
        // 6. Confirmer la commande
        cy.get('[data-testid="place-order"]').click();
        
        // Attendre le traitement
        cy.get('[data-testid="loading"]', { timeout: 10000 }).should('not.exist');
        
        // 7. Vérifier la confirmation
        cy.url().should('include', '/order-confirmation');
        cy.get('[data-testid="order-number"]').should('exist');
        cy.get('[data-testid="success-message"]').should('contain', 'Order placed successfully');
        
        // Vérifier que l'email a été envoyé (via API)
        cy.request('/api/test/emails').then((response) => {
            expect(response.body).to.have.length(1);
            expect(response.body[0].to).to.equal('user@example.com');
            expect(response.body[0].subject).to.contain('Order Confirmation');
        });
    });
    
    it('devrait gérer les erreurs de paiement', () => {
        // Connexion et ajout au panier (steps précédents)
        // ...
        
        // Utiliser une carte qui décline
        cy.get('[data-testid="card-number"]').type('4000000000000002');
        cy.get('[data-testid="place-order"]').click();
        
        // Vérifier le message d'erreur
        cy.get('[data-testid="error-message"]')
            .should('be.visible')
            .and('contain', 'Payment declined');
        
        // Vérifier qu'on est toujours sur la page checkout
        cy.url().should('include', '/checkout');
    });
});
```

**Exemple Selenium (Tests E2E):**

```java
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
class OrderE2ETest {
    
    @LocalServerPort
    private int port;
    
    private WebDriver driver;
    private String baseUrl;
    
    @BeforeEach
    void setUp() {
        driver = new ChromeDriver();
        baseUrl = "http://localhost:" + port;
    }
    
    @AfterEach
    void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
    
    @Test
    void completeOrderFlow() {
        // 1. Navigation vers la page de connexion
        driver.get(baseUrl + "/login");
        
        // 2. Connexion
        driver.findElement(By.id("email")).sendKeys("user@example.com");
        driver.findElement(By.id("password")).sendKeys("password123");
        driver.findElement(By.id("login-btn")).click();
        
        // Attendre le chargement
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        wait.until(ExpectedConditions.urlContains("/dashboard"));
        
        // 3. Rechercher un produit
        driver.findElement(By.id("search")).sendKeys("laptop");
        driver.findElement(By.id("search-btn")).click();
        
        wait.until(ExpectedConditions.presenceOfElementLocated(By.className("product-card")));
        
        // 4. Ajouter au panier
        driver.findElement(By.className("add-to-cart")).click();
        
        // Vérifier la notification
        wait.until(ExpectedConditions.visibilityOfElementLocated(By.className("notification")));
        String notification = driver.findElement(By.className("notification")).getText();
        assertTrue(notification.contains("Added to cart"));
        
        // 5. Aller au panier et commander
        driver.findElement(By.id("cart-icon")).click();
        wait.until(ExpectedConditions.urlContains("/cart"));
        
        driver.findElement(By.id("checkout-btn")).click();
        
        // 6. Paiement
        wait.until(ExpectedConditions.urlContains("/checkout"));
        driver.findElement(By.id("card-number")).sendKeys("4242424242424242");
        driver.findElement(By.id("card-expiry")).sendKeys("1225");
        driver.findElement(By.id("card-cvc")).sendKeys("123");
        
        driver.findElement(By.id("place-order-btn")).click();
        
        // 7. Vérifier la confirmation
        wait.until(ExpectedConditions.urlContains("/order-confirmation"));
        String confirmation = driver.findElement(By.className("success-message")).getText();
        assertTrue(confirmation.contains("Order placed successfully"));
    }
}
```

---

## 6.2 Stratégies de test : TDD, BDD

### 6.2.1 TDD (Test-Driven Development)

**Cycle Red-Green-Refactor:**

```
1. RED: Écrire un test qui échoue
         ↓
2. GREEN: Écrire le code minimum pour passer le test
         ↓
3. REFACTOR: Améliorer le code sans casser les tests
         ↓
   Répéter
```

**Exemple pratique TDD:**

```java
// ÉTAPE 1: RED - Écrire le test d'abord
@Test
void shouldCalculateDiscountForRegularCustomer() {
    // Arrange
    DiscountCalculator calculator = new DiscountCalculator();
    BigDecimal orderTotal = new BigDecimal("100.00");
    CustomerType type = CustomerType.REGULAR;
    
    // Act
    BigDecimal discount = calculator.calculate(orderTotal, type);
    
    // Assert
    assertEquals(new BigDecimal("5.00"), discount);  // 5% pour REGULAR
}

// Le test échoue car DiscountCalculator n'existe pas encore
// Compilation error!

// ÉTAPE 2: GREEN - Code minimum pour passer le test
public class DiscountCalculator {
    public BigDecimal calculate(BigDecimal orderTotal, CustomerType type) {
        return orderTotal.multiply(new BigDecimal("0.05"));
    }
}

// Le test passe maintenant! ✓

// ÉTAPE 3: RED - Ajouter un nouveau test
@Test
void shouldCalculateDiscountForVIPCustomer() {
    // Arrange
    DiscountCalculator calculator = new DiscountCalculator();
    BigDecimal orderTotal = new BigDecimal("100.00");
    CustomerType type = CustomerType.VIP;
    
    // Act
    BigDecimal discount = calculator.calculate(orderTotal, type);
    
    // Assert
    assertEquals(new BigDecimal("10.00"), discount);  // 10% pour VIP
}

// Ce test échoue (retourne 5.00 au lieu de 10.00)

// ÉTAPE 4: GREEN - Modifier le code
public class DiscountCalculator {
    public BigDecimal calculate(BigDecimal orderTotal, CustomerType type) {
        BigDecimal rate = switch (type) {
            case REGULAR -> new BigDecimal("0.05");
            case VIP -> new BigDecimal("0.10");
            default -> BigDecimal.ZERO;
        };
        return orderTotal.multiply(rate);
    }
}

// Les deux tests passent maintenant! ✓✓

// ÉTAPE 5: REFACTOR - Améliorer le code
public class DiscountCalculator {
    
    private static final Map<CustomerType, BigDecimal> DISCOUNT_RATES = Map.of(
        CustomerType.REGULAR, new BigDecimal("0.05"),
        CustomerType.VIP, new BigDecimal("0.10"),
        CustomerType.GOLD, new BigDecimal("0.15")
    );
    
    public BigDecimal calculate(BigDecimal orderTotal, CustomerType type) {
        BigDecimal rate = DISCOUNT_RATES.getOrDefault(type, BigDecimal.ZERO);
        return orderTotal.multiply(rate);
    }
}

// Les tests passent toujours! ✓✓
// Le code est plus maintenable et extensible
```

**Avantages du TDD:**
- ✅ Design émergent (le code est testable par design)
- ✅ Documentation vivante (les tests montrent l'usage)
- ✅ Confiance dans le refactoring
- ✅ Moins de bugs
- ✅ Feedback rapide

**Inconvénients:**
- ❌ Courbe d'apprentissage
- ❌ Peut sembler plus lent initialement
- ❌ Difficile sur code legacy
- ❌ Peut mener à over-engineering

### 6.2.2 BDD (Behavior-Driven Development)

**Principe:** Décrire le comportement attendu en langage naturel compréhensible par tous (métier + tech).

**Format Gherkin:**

```gherkin
Feature: Gestion du panier d'achat
  En tant que client
  Je veux pouvoir ajouter des produits à mon panier
  Afin de les acheter plus tard

  Background:
    Given un utilisateur connecté "john@example.com"
    And un catalogue de produits disponibles

  Scenario: Ajouter un produit au panier
    Given je suis sur la page du produit "Laptop Dell XPS 15"
    When je clique sur "Ajouter au panier"
    Then le produit devrait être dans mon panier
    And le compteur du panier devrait afficher "1"
    And une notification de succès devrait s'afficher

  Scenario: Ajouter plusieurs unités d'un produit
    Given je suis sur la page du produit "Mouse Logitech"
    And je sélectionne la quantité "3"
    When je clique sur "Ajouter au panier"
    Then le panier devrait contenir "3" unités de "Mouse Logitech"

  Scenario: Ajouter un produit en rupture de stock
    Given je suis sur la page du produit "Clavier Mécanique"
    And le produit est en rupture de stock
    When je clique sur "Ajouter au panier"
    Then un message d'erreur devrait s'afficher
    And le produit ne devrait pas être ajouté au panier

  Scenario Outline: Calculer le total du panier
    Given mon panier contient:
      | Produit     | Prix  | Quantité |
      | <produit1>  | <prix1> | <qte1> |
      | <produit2>  | <prix2> | <qte2> |
    Then le total devrait être "<total>"

    Examples:
      | produit1 | prix1 | qte1 | produit2 | prix2 | qte2 | total  |
      | Laptop   | 1000  | 1    | Mouse    | 50    | 2    | 1100   |
      | Keyboard | 150   | 2    | Monitor  | 300   | 1    | 600    |
```

**Implémentation avec Cucumber (Java):**

```java
// Step Definitions
public class CartSteps {
    
    private User currentUser;
    private Product currentProduct;
    private Cart cart;
    private String notificationMessage;
    
    @Given("un utilisateur connecté {string}")
    public void unUtilisateurConnecte(String email) {
        currentUser = userRepository.findByEmail(email);
        cart = cartService.getCartForUser(currentUser);
    }
    
    @Given("un catalogue de produits disponibles")
    public void unCatalogueDeProduits() {
        // Les produits sont déjà en DB (via @BeforeAll)
    }
    
    @Given("je suis sur la page du produit {string}")
    public void jeSuisSurLaPageDuProduit(String productName) {
        currentProduct = productRepository.findByName(productName);
        assertNotNull(currentProduct, "Product should exist");
    }
    
    @Given("le produit est en rupture de stock")
    public void leProduitEstEnRuptureDeStock() {
        currentProduct.setStock(0);
    }
    
    @Given("je sélectionne la quantité {string}")
    public void jeSelectionne LaCant(String quantity) {
        cart.setSelectedQuantity(Integer.parseInt(quantity));
    }
    
    @When("je clique sur {string}")
    public void jeCliqueSur(String buttonText) {
        try {
            if (buttonText.equals("Ajouter au panier")) {
                cart.addProduct(currentProduct, cart.getSelectedQuantity());
                notificationMessage = "Product added to cart";
            }
        } catch (OutOfStockException e) {
            notificationMessage = e.getMessage();
        }
    }
    
    @Then("le produit devrait être dans mon panier")
    public void leProduitDevraitEtreDansMonPanier() {
        assertTrue(cart.contains(currentProduct));
    }
    
    @Then("le compteur du panier devrait afficher {string}")
    public void leCompteurDuPanierDevraitAfficher(String count) {
        assertEquals(Integer.parseInt(count), cart.getItemCount());
    }
    
    @Then("une notification de succès devrait s'afficher")
    public void uneNotificationDeSuccesDevraitSafficher() {
        assertNotNull(notificationMessage);
        assertTrue(notificationMessage.contains("success") || 
                   notificationMessage.contains("added"));
    }
    
    @Then("le panier devrait contenir {string} unités de {string}")
    public void lePanierDevraitContenirUnitesDe(String quantity, String productName) {
        CartItem item = cart.getItemByProductName(productName);
        assertNotNull(item);
        assertEquals(Integer.parseInt(quantity), item.getQuantity());
    }
    
    @Then("un message d'erreur devrait s'afficher")
    public void unMessageDerreurDevraitSafficher() {
        assertNotNull(notificationMessage);
        assertTrue(notificationMessage.toLowerCase().contains("error") ||
                   notificationMessage.toLowerCase().contains("stock"));
    }
    
    @Given("mon panier contient:")
    public void monPanierContient(io.cucumber.datatable.DataTable dataTable) {
        List<Map<String, String>> rows = dataTable.asMaps();
        for (Map<String, String> row : rows) {
            Product product = new Product(row.get("Produit"), 
                                        new BigDecimal(row.get("Prix")));
            int quantity = Integer.parseInt(row.get("Quantité"));
            cart.addProduct(product, quantity);
        }
    }
    
    @Then("le total devrait être {string}")
    public void leTotalDevraitEtre(String expectedTotal) {
        BigDecimal total = cart.calculateTotal();
        assertEquals(new BigDecimal(expectedTotal), total);
    }
}
```

**Exemple SpecFlow (C# / .NET):**

```csharp
[Binding]
public class CartSteps
{
    private readonly ScenarioContext _scenarioContext;
    private readonly CartService _cartService;
    private Cart _cart;
    private string _notificationMessage;
    
    public CartSteps(ScenarioContext scenarioContext, CartService cartService)
    {
        _scenarioContext = scenarioContext;
        _cartService = cartService;
    }
    
    [Given(@"un utilisateur connecté ""(.*)""")]
    public void GivenUnUtilisateurConnecte(string email)
    {
        var user = _userRepository.FindByEmail(email);
        _cart = _cartService.GetCartForUser(user);
        _scenarioContext["cart"] = _cart;
    }
    
    [When(@"je clique sur ""Ajouter au panier""")]
    public void WhenJeCliqueSurAjouterAuPanier()
    {
        try
        {
            var product = _scenarioContext.Get<Product>("currentProduct");
            _cart.AddProduct(product);
            _notificationMessage = "Product added";
        }
        catch (OutOfStockException ex)
        {
            _notificationMessage = ex.Message;
        }
    }
    
    [Then(@"le produit devrait être dans mon panier")]
    public void ThenLeProduitDevraitEtreDansMonPanier()
    {
        var product = _scenarioContext.Get<Product>("currentProduct");
        Assert.That(_cart.Contains(product), Is.True);
    }
}
```

---

## 6.3 Outils de test (JUnit, PyTest, Jest, Postman)

### 6.3.1 Tests d'API avec Postman

**Collection Postman:**

```json
{
  "info": {
    "name": "E-commerce API",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "item": [
    {
      "name": "Authentication",
      "item": [
        {
          "name": "Login",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test('Status code is 200', function () {",
                  "    pm.response.to.have.status(200);",
                  "});",
                  "",
                  "pm.test('Response has token', function () {",
                  "    var jsonData = pm.response.json();",
                  "    pm.expect(jsonData).to.have.property('token');",
                  "    pm.environment.set('authToken', jsonData.token);",
                  "});",
                  "",
                  "pm.test('Token is valid format', function () {",
                  "    var jsonData = pm.response.json();",
                  "    pm.expect(jsonData.token).to.match(/^[A-Za-z0-9-_]+\\.[A-Za-z0-9-_]+\\.[A-Za-z0-9-_]+$/);",
                  "});"
                ]
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"email\": \"{{userEmail}}\",\n  \"password\": \"{{userPassword}}\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/auth/login",
              "host": ["{{baseUrl}}"],
              "path": ["api", "auth", "login"]
            }
          }
        }
      ]
    },
    {
      "name": "Products",
      "item": [
        {
          "name": "Get All Products",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test('Status code is 200', () => {",
                  "    pm.response.to.have.status(200);",
                  "});",
                  "",
                  "pm.test('Response is array', () => {",
                  "    var jsonData = pm.response.json();",
                  "    pm.expect(jsonData.data).to.be.an('array');",
                  "});",
                  "",
                  "pm.test('Products have required fields', () => {",
                  "    var products = pm.response.json().data;",
                  "    products.forEach(product => {",
                  "        pm.expect(product).to.have.property('id');",
                  "        pm.expect(product).to.have.property('name');",
                  "        pm.expect(product).to.have.property('price');",
                  "    });",
                  "});",
                  "",
                  "pm.test('Response time is less than 500ms', () => {",
                  "    pm.expect(pm.response.responseTime).to.be.below(500);",
                  "});"
                ]
              }
            }
          ],
          "request": {
            "method": "GET",
            "header": [
              {
                "key": "Authorization",
                "value": "Bearer {{authToken}}"
              }
            ],
            "url": {
              "raw": "{{baseUrl}}/api/products?page=1&limit=20",
              "host": ["{{baseUrl}}"],
              "path": ["api", "products"],
              "query": [
                {"key": "page", "value": "1"},
                {"key": "limit", "value": "20"}
              ]
            }
          }
        },
        {
          "name": "Create Product",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test('Status code is 201', () => {",
                  "    pm.response.to.have.status(201);",
                  "});",
                  "",
                  "pm.test('Product created with ID', () => {",
                  "    var jsonData = pm.response.json();",
                  "    pm.expect(jsonData).to.have.property('id');",
                  "    pm.environment.set('lastProductId', jsonData.id);",
                  "});",
                  "",
                  "pm.test('Created product matches request', () => {",
                  "    var jsonData = pm.response.json();",
                  "    var request = JSON.parse(pm.request.body.raw);",
                  "    pm.expect(jsonData.name).to.equal(request.name);",
                  "    pm.expect(jsonData.price).to.equal(request.price);",
                  "});"
                ]
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Authorization",
                "value": "Bearer {{authToken}}"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"name\": \"Test Product\",\n  \"price\": 99.99,\n  \"description\": \"Test description\",\n  \"category\": \"Electronics\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/products",
              "host": ["{{baseUrl}}"],
              "path": ["api", "products"]
            }
          }
        }
      ]
    }
  ]
}
```

**Newman (CLI Postman):**

```bash
# Installer Newman
npm install -g newman

# Exécuter une collection
newman run collection.json \
  --environment environment.json \
  --reporters cli,html \
  --reporter-html-export report.html

# Intégration CI/CD
newman run collection.json \
  --environment prod.json \
  --bail \
  --color off
```

### 6.3.2 Tests de performance

**JMeter:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jmeterTestPlan version="1.2">
  <hashTree>
    <TestPlan guiclass="TestPlanGui" testclass="TestPlan" testname="API Load Test">
      <elementProp name="TestPlan.user_defined_variables" elementType="Arguments">
        <collectionProp name="Arguments.arguments">
          <elementProp name="BASE_URL" elementType="Argument">
            <stringProp name="Argument.name">BASE_URL</stringProp>
            <stringProp name="Argument.value">https://api.myapp.com</stringProp>
          </elementProp>
        </collectionProp>
      </elementProp>
    </TestPlan>
    
    <hashTree>
      <ThreadGroup guiclass="ThreadGroupGui" testclass="ThreadGroup" testname="Users">
        <stringProp name="ThreadGroup.num_threads">100</stringProp>
        <stringProp name="ThreadGroup.ramp_time">10</stringProp>
        <stringProp name="ThreadGroup.duration">300</stringProp>
      </ThreadGroup>
      
      <hashTree>
        <HTTPSamplerProxy guiclass="HttpTestSampleGui" testclass="HTTPSamplerProxy" testname="Get Products">
          <stringProp name="HTTPSampler.domain">${BASE_URL}</stringProp>
          <stringProp name="HTTPSampler.path">/api/products</stringProp>
          <stringProp name="HTTPSampler.method">GET</stringProp>
        </HTTPSamplerProxy>
        
        <ResponseAssertion guiclass="AssertionGui" testclass="ResponseAssertion">
          <stringProp name="Assertion.test_field">Assertion.response_code</stringProp>
          <stringProp name="Assertion.test_type">8</stringProp>
          <stringProp name="Assertion.custom_message"></stringProp>
          <collectionProp name="Asserion.test_strings">
            <stringProp name="49586">200</stringProp>
          </collectionProp>
        </ResponseAssertion>
        
        <DurationAssertion guiclass="DurationAssertionGui" testclass="DurationAssertion">
          <stringProp name="DurationAssertion.duration">500</stringProp>
        </DurationAssertion>
      </hashTree>
    </hashTree>
  </hashTree>
</jmeterTestPlan>
```

**k6 (Tests de charge modernes):**

```javascript
// load-test.js
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate } from 'k6/metrics';

const errorRate = new Rate('errors');

export const options = {
    stages: [
        { duration: '30s', target: 20 },   // Ramp-up
        { duration: '1m', target: 100 },   // Peak
        { duration: '30s', target: 0 },    // Ramp-down
    ],
    thresholds: {
        http_req_duration: ['p(95)<500'],  // 95% des requêtes < 500ms
        errors: ['rate<0.1'],              // < 10% d'erreurs
    },
};

export default function () {
    // 1. Login
    const loginRes = http.post('https://api.myapp.com/auth/login', JSON.stringify({
        email: 'test@example.com',
        password: 'password123',
    }), {
        headers: { 'Content-Type': 'application/json' },
    });
    
    check(loginRes, {
        'login status is 200': (r) => r.status === 200,
        'login has token': (r) => r.json('token') !== '',
    }) || errorRate.add(1);
    
    const token = loginRes.json('token');
    
    sleep(1);
    
    // 2. Get products
    const productsRes = http.get('https://api.myapp.com/api/products', {
        headers: {
            'Authorization': `Bearer ${token}`,
        },
    });
    
    check(productsRes, {
        'products status is 200': (r) => r.status === 200,
        'products response time < 500ms': (r) => r.timings.duration < 500,
        'products has data': (r) => r.json('data').length > 0,
    }) || errorRate.add(1);
    
    sleep(2);
    
    // 3. Create order
    const orderRes = http.post('https://api.myapp.com/api/orders', JSON.stringify({
        items: [
            { productId: 1, quantity: 2 }
        ]
    }), {
        headers: {
            'Authorization': `Bearer ${token}`,
            'Content-Type': 'application/json',
        },
    });
    
    check(orderRes, {
        'order created': (r) => r.status === 201,
        'order has ID': (r) => r.json('id') !== null,
    }) || errorRate.add(1);
    
    sleep(1);
}

// Exécution:
// k6 run --vus 100 --duration 5m load-test.js
```

---

## 6.4 Mesure de la couverture et validation logicielle

### 6.4.1 Couverture de code (Code Coverage)

**Types de couverture:**

1. **Line Coverage:** % de lignes exécutées
2. **Branch Coverage:** % de branches (if/else) testées
3. **Function Coverage:** % de fonctions appelées
4. **Statement Coverage:** % d'instructions exécutées

**Exemple JaCoCo (Java):**

```xml
<!-- pom.xml -->
<build>
    <plugins>
        <plugin>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>0.8.10</version>
            <executions>
                <execution>
                    <goals>
                        <goal>prepare-agent</goal>
                    </goals>
                </execution>
                <execution>
                    <id>report</id>
                    <phase>test</phase>
                    <goals>
                        <goal>report</goal>
                    </goals>
                </execution>
                <execution>
                    <id>jacoco-check</id>
                    <goals>
                        <goal>check</goal>
                    </goals>
                    <configuration>
                        <rules>
                            <rule>
                                <element>PACKAGE</element>
                                <limits>
                                    <limit>
                                        <counter>LINE</counter>
                                        <value>COVEREDRATIO</value>
                                        <minimum>0.80</minimum>
                                    </limit>
                                    <limit>
                                        <counter>BRANCH</counter>
                                        <value>COVEREDRATIO</value>
                                        <minimum>0.75</minimum>
                                    </limit>
                                </limits>
                            </rule>
                        </rules>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

```bash
# Exécuter les tests avec coverage
mvn clean test jacoco:report

# Rapport disponible dans:
# target/site/jacoco/index.html
```

**Exemple Istanbul/NYC (JavaScript):**

```json
// package.json
{
  "scripts": {
    "test": "jest",
    "test:coverage": "jest --coverage",
    "test:coverage:watch": "jest --coverage --watchAll"
  },
  "jest": {
    "coverageThreshold": {
      "global": {
        "branches": 75,
        "functions": 80,
        "lines": 80,
        "statements": 80
      }
    },
    "collectCoverageFrom": [
      "src/**/*.{js,ts}",
      "!src/**/*.test.{js,ts}",
      "!src/**/*.spec.{js,ts}",
      "!src/index.ts"
    ]
  }
}
```

```bash
# Exécuter avec coverage
npm run test:coverage

# Rapport HTML dans: coverage/lcov-report/index.html
```

**Interprétation:**

```
✅ 80-100%: Excellente couverture
⚠️  60-80%: Couverture acceptable
❌ < 60%: Couverture insuffisante

Attention: 100% de couverture ≠ 0 bug!
La couverture mesure ce qui est exécuté, pas ce qui est testé correctement.
```

### 6.4.2 Analyse statique du code

**SonarQube:**

```yaml
# sonar-project.properties
sonar.projectKey=my-project
sonar.projectName=My Project
sonar.projectVersion=1.0

sonar.sources=src
sonar.tests=src
sonar.test.inclusions=**/*.test.js,**/*.spec.js

sonar.javascript.lcov.reportPaths=coverage/lcov.info

sonar.exclusions=**/node_modules/**,**/dist/**

# Quality gates
sonar.qualitygate.wait=true
```

```bash
# Scanner
sonar-scanner
```

**ESLint (JavaScript):**

```javascript
// .eslintrc.js
module.exports = {
  extends: ['eslint:recommended', 'plugin:@typescript-eslint/recommended'],
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  root: true,
  rules: {
    'no-console': 'warn',
    'no-unused-vars': 'error',
    'complexity': ['error', 10],
    'max-lines-per-function': ['warn', 50],
    'max-depth': ['error', 4],
  },
};
```

### 6.4.3 Mutation Testing

**Principe:** Introduire des mutations (bugs) dans le code et vérifier que les tests les détectent.

**PITest (Java):**

```xml
<plugin>
    <groupId>org.pitest</groupId>
    <artifactId>pitest-maven</artifactId>
    <version>1.14.1</version>
    <configuration>
        <targetClasses>
            <param>com.myapp.*</param>
        </targetClasses>
        <targetTests>
            <param>com.myapp.*Test</param>
        </targetTests>
        <mutationThreshold>80</mutationThreshold>
    </configuration>
</plugin>
```

```bash
mvn org.pitest:pitest-maven:mutationCoverage
```

**Stryker (JavaScript):**

```javascript
// stryker.conf.js
module.exports = {
  mutate: ['src/**/*.ts', '!src/**/*.spec.ts'],
  testRunner: 'jest',
  coverageAnalysis: 'perTest',
  thresholds: { high: 80, low: 60, break: 50 },
};
```

---

## 📝 Résumé du chapitre

✅ **Pyramide des tests:** 70% unitaires, 20% intégration, 10% E2E

✅ **TDD:** Red-Green-Refactor pour un code testable par design

✅ **BDD:** Spécification par l'exemple en langage naturel (Gherkin)

✅ **Outils:** JUnit, Jest, PyTest, Postman, JMeter, k6

✅ **Couverture:** Mesure quantitative, mais qualité prime

✅ **Analyse statique:** SonarQube, ESLint pour détecter problèmes

---

## 💡 Exercices pratiques

### Exercice 1: TDD
Implémentez une calculatrice en TDD (addition, soustraction, multiplication, division).

### Exercice 2: BDD
Écrivez des scénarios Gherkin pour un système de connexion utilisateur.

### Exercice 3: Tests d'API
Créez une collection Postman avec tests automatisés pour une API REST.

---

## 📚 Pour aller plus loin

- 📖 **"Test-Driven Development"** - Kent Beck
- 📖 **"The BDD Books"** - Gaspar Nagy & Seb Rose
- 📖 **"Growing Object-Oriented Software, Guided by Tests"** - Freeman & Pryce
- 🔗 **Cucumber.io**: Documentation BDD
- 🔗 **k6.io**: Tests de performance modernes

---

[⬅️ Chapitre 5](./chapitre-5-developpement-collaboratif.md) | [➡️ Partie III](../partie-3-technologies-frameworks/README.md)

