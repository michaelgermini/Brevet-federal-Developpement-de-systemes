# Chapitre 8 : Développement desktop et mobile

> **Objectif:** Maîtriser le développement d'applications desktop et mobiles, comprendre les approches natives et multiplateformes, et utiliser les frameworks modernes.

---

## 📋 Table des matières

1. [Applications natives et multiplateformes](#81-applications-natives-et-multiplateformes)
2. [Frameworks modernes](#82-frameworks-modernes-qt-electron-flutter-react-native)
3. [Gestion des performances et ressources](#83-gestion-des-performances-et-ressources)
4. [Connexion aux services cloud et aux API](#84-connexion-aux-services-cloud-et-aux-api)

---

## 8.1 Applications natives et multiplateformes

### 8.1.1 Native vs Multiplateforme

**Applications Natives:**

```
iOS                     Android                 Desktop
Swift/Objective-C       Kotlin/Java            C++/C#
Xcode                   Android Studio          Platform SDKs
UIKit/SwiftUI          Jetpack Compose         WPF/Qt

✅ Performance maximale
✅ Accès complet aux API natives
✅ UX optimale pour la plateforme
❌ Code séparé par plateforme
❌ Coût de développement élevé
❌ Maintenance complexe
```

**Applications Multiplateformes:**

```
React Native            Flutter                Electron
JavaScript/TypeScript   Dart                   Web Technologies
Metro Bundler           Dart VM                Chromium + Node.js
iOS + Android          iOS + Android + Web     Windows/Mac/Linux

✅ Code partagé (60-90%)
✅ Développement rapide
✅ Équipe unique
❌ Performance parfois inférieure
❌ Limitations d'accès natif
❌ Taille app plus importante
```

| Critère | Native | Multiplateforme |
|---------|--------|-----------------|
| **Performance** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **UX** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Time-to-market** | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Coût** | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Maintenance** | ⭐⭐ | ⭐⭐⭐⭐ |
| **Accès API** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |

---

## 8.2 Frameworks modernes (Qt, Electron, Flutter, React Native)

### 8.2.1 Flutter

**Architecture:**

```
┌──────────────────────────────────┐
│      Your App (Dart)             │
├──────────────────────────────────┤
│    Flutter Framework             │
│  - Widgets                       │
│  - Rendering                     │
│  - Animation                     │
├──────────────────────────────────┤
│    Flutter Engine (C/C++)        │
│  - Skia (2D rendering)           │
│  - Dart runtime                  │
├──────────────────────────────────┤
│   Platform-specific Embedder     │
│  - iOS / Android / Web / Desktop │
└──────────────────────────────────┘
```

**Exemple d'application Flutter:**

```dart
// main.dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Products App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: ProductListPage(),
    );
  }
}

// Product Model
class Product {
  final int id;
  final String name;
  final double price;
  final String imageUrl;

  Product({
    required this.id,
    required this.name,
    required this.price,
    required this.imageUrl,
  });

  factory Product.fromJson(Map<String, dynamic> json) {
    return Product(
      id: json['id'],
      name: json['name'],
      price: json['price'].toDouble(),
      imageUrl: json['imageUrl'] ?? '',
    );
  }
}

// Product List Page
class ProductListPage extends StatefulWidget {
  @override
  _ProductListPageState createState() => _ProductListPageState();
}

class _ProductListPageState extends State<ProductListPage> {
  late Future<List<Product>> futureProducts;

  @override
  void initState() {
    super.initState();
    futureProducts = fetchProducts();
  }

  Future<List<Product>> fetchProducts() async {
    final response = await http.get(
      Uri.parse('https://api.example.com/products'),
    );

    if (response.statusCode == 200) {
      List<dynamic> data = json.decode(response.body);
      return data.map((json) => Product.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load products');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Products'),
        actions: [
          IconButton(
            icon: Icon(Icons.shopping_cart),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(builder: (context) => CartPage()),
              );
            },
          ),
        ],
      ),
      body: FutureBuilder<List<Product>>(
        future: futureProducts,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return Center(child: CircularProgressIndicator());
          } else if (snapshot.hasError) {
            return Center(child: Text('Error: ${snapshot.error}'));
          } else if (!snapshot.hasData || snapshot.data!.isEmpty) {
            return Center(child: Text('No products found'));
          }

          return ListView.builder(
            itemCount: snapshot.data!.length,
            itemBuilder: (context, index) {
              final product = snapshot.data![index];
              return ProductCard(product: product);
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          setState(() {
            futureProducts = fetchProducts();
          });
        },
        child: Icon(Icons.refresh),
      ),
    );
  }
}

// Product Card Widget
class ProductCard extends StatelessWidget {
  final Product product;

  const ProductCard({Key? key, required this.product}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: EdgeInsets.all(8),
      child: ListTile(
        leading: Image.network(
          product.imageUrl,
          width: 50,
          height: 50,
          fit: BoxFit.cover,
          errorBuilder: (context, error, stackTrace) {
            return Icon(Icons.image_not_supported);
          },
        ),
        title: Text(product.name),
        subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
        trailing: IconButton(
          icon: Icon(Icons.add_shopping_cart),
          onPressed: () {
            ScaffoldMessenger.of(context).showSnackBar(
              SnackBar(
                content: Text('${product.name} added to cart'),
                duration: Duration(seconds: 2),
              ),
            );
          },
        ),
        onTap: () {
          Navigator.push(
            context,
            MaterialPageRoute(
              builder: (context) => ProductDetailPage(product: product),
            ),
          );
        },
      ),
    );
  }
}

// Product Detail Page
class ProductDetailPage extends StatelessWidget {
  final Product product;

  const ProductDetailPage({Key? key, required this.product}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(product.name)),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Image.network(
              product.imageUrl,
              width: double.infinity,
              height: 300,
              fit: BoxFit.cover,
            ),
            SizedBox(height: 16),
            Text(
              product.name,
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            SizedBox(height: 8),
            Text(
              '\$${product.price.toStringAsFixed(2)}',
              style: Theme.of(context).textTheme.headlineMedium?.copyWith(
                    color: Colors.green,
                    fontWeight: FontWeight.bold,
                  ),
            ),
            SizedBox(height: 16),
            ElevatedButton.icon(
              onPressed: () {
                // Add to cart logic
              },
              icon: Icon(Icons.shopping_cart),
              label: Text('Add to Cart'),
              style: ElevatedButton.styleFrom(
                minimumSize: Size(double.infinity, 50),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

**State Management avec Provider:**

```dart
// pubspec.yaml
dependencies:
  provider: ^6.0.0

// cart_provider.dart
import 'package:flutter/foundation.dart';

class CartItem {
  final Product product;
  int quantity;

  CartItem({required this.product, this.quantity = 1});
}

class CartProvider with ChangeNotifier {
  final List<CartItem> _items = [];

  List<CartItem> get items => _items;

  int get itemCount => _items.fold(0, (sum, item) => sum + item.quantity);

  double get total => _items.fold(
      0, (sum, item) => sum + (item.product.price * item.quantity));

  void addItem(Product product) {
    final existingIndex = _items.indexWhere(
      (item) => item.product.id == product.id,
    );

    if (existingIndex >= 0) {
      _items[existingIndex].quantity++;
    } else {
      _items.add(CartItem(product: product));
    }

    notifyListeners();
  }

  void removeItem(int productId) {
    _items.removeWhere((item) => item.product.id == productId);
    notifyListeners();
  }

  void clear() {
    _items.clear();
    notifyListeners();
  }
}

// main.dart (avec Provider)
void main() {
  runApp(
    ChangeNotifierProvider(
      create: (context) => CartProvider(),
      child: MyApp(),
    ),
  );
}

// Utilisation
Consumer<CartProvider>(
  builder: (context, cart, child) {
    return Text('Items: ${cart.itemCount}');
  },
)
```

### 8.2.2 React Native

**Exemple d'application:**

```javascript
// App.js
import React, { useState, useEffect } from 'react';
import {
  SafeAreaView,
  ScrollView,
  StatusBar,
  StyleSheet,
  Text,
  View,
  FlatList,
  Image,
  TouchableOpacity,
  ActivityIndicator,
} from 'react-native';

// Product List Screen
const ProductListScreen = ({ navigation }) => {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetchProducts();
  }, []);

  const fetchProducts = async () => {
    try {
      const response = await fetch('https://api.example.com/products');
      const data = await response.json();
      setProducts(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  const renderProduct = ({ item }) => (
    <TouchableOpacity
      style={styles.productCard}
      onPress={() => navigation.navigate('ProductDetail', { product: item })}
    >
      <Image
        source={{ uri: item.imageUrl }}
        style={styles.productImage}
        resizeMode="cover"
      />
      <View style={styles.productInfo}>
        <Text style={styles.productName}>{item.name}</Text>
        <Text style={styles.productPrice}>${item.price.toFixed(2)}</Text>
      </View>
      <TouchableOpacity
        style={styles.addButton}
        onPress={() => addToCart(item)}
      >
        <Text style={styles.addButtonText}>+</Text>
      </TouchableOpacity>
    </TouchableOpacity>
  );

  if (loading) {
    return (
      <View style={styles.centerContainer}>
        <ActivityIndicator size="large" color="#0000ff" />
      </View>
    );
  }

  if (error) {
    return (
      <View style={styles.centerContainer}>
        <Text style={styles.errorText}>Error: {error}</Text>
      </View>
    );
  }

  return (
    <SafeAreaView style={styles.container}>
      <StatusBar barStyle="dark-content" />
      <FlatList
        data={products}
        renderItem={renderProduct}
        keyExtractor={(item) => item.id.toString()}
        contentContainerStyle={styles.listContainer}
      />
    </SafeAreaView>
  );
};

// Product Detail Screen
const ProductDetailScreen = ({ route }) => {
  const { product } = route.params;

  return (
    <ScrollView style={styles.container}>
      <Image
        source={{ uri: product.imageUrl }}
        style={styles.detailImage}
        resizeMode="cover"
      />
      <View style={styles.detailContent}>
        <Text style={styles.detailName}>{product.name}</Text>
        <Text style={styles.detailPrice}>${product.price.toFixed(2)}</Text>
        <Text style={styles.detailDescription}>{product.description}</Text>
        <TouchableOpacity style={styles.cartButton}>
          <Text style={styles.cartButtonText}>Add to Cart</Text>
        </TouchableOpacity>
      </View>
    </ScrollView>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
  centerContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  listContainer: {
    padding: 16,
  },
  productCard: {
    flexDirection: 'row',
    backgroundColor: '#f9f9f9',
    borderRadius: 8,
    padding: 12,
    marginBottom: 12,
    alignItems: 'center',
  },
  productImage: {
    width: 60,
    height: 60,
    borderRadius: 8,
  },
  productInfo: {
    flex: 1,
    marginLeft: 12,
  },
  productName: {
    fontSize: 16,
    fontWeight: '600',
    marginBottom: 4,
  },
  productPrice: {
    fontSize: 14,
    color: '#666',
  },
  addButton: {
    width: 32,
    height: 32,
    borderRadius: 16,
    backgroundColor: '#007AFF',
    justifyContent: 'center',
    alignItems: 'center',
  },
  addButtonText: {
    color: '#fff',
    fontSize: 20,
    fontWeight: 'bold',
  },
  detailImage: {
    width: '100%',
    height: 300,
  },
  detailContent: {
    padding: 16,
  },
  detailName: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 8,
  },
  detailPrice: {
    fontSize: 20,
    color: '#28a745',
    fontWeight: '600',
    marginBottom: 16,
  },
  detailDescription: {
    fontSize: 16,
    lineHeight: 24,
    color: '#333',
    marginBottom: 24,
  },
  cartButton: {
    backgroundColor: '#007AFF',
    padding: 16,
    borderRadius: 8,
    alignItems: 'center',
  },
  cartButtonText: {
    color: '#fff',
    fontSize: 18,
    fontWeight: '600',
  },
});

export default ProductListScreen;
```

**Redux pour state management:**

```javascript
// store/cartSlice.js
import { createSlice } from '@reduxjs/toolkit';

const cartSlice = createSlice({
  name: 'cart',
  initialState: {
    items: [],
  },
  reducers: {
    addToCart: (state, action) => {
      const existingItem = state.items.find(
        item => item.id === action.payload.id
      );
      
      if (existingItem) {
        existingItem.quantity += 1;
      } else {
        state.items.push({ ...action.payload, quantity: 1 });
      }
    },
    removeFromCart: (state, action) => {
      state.items = state.items.filter(item => item.id !== action.payload);
    },
    clearCart: (state) => {
      state.items = [];
    },
  },
});

export const { addToCart, removeFromCart, clearCart } = cartSlice.actions;
export default cartSlice.reducer;

// store/index.js
import { configureStore } from '@reduxjs/toolkit';
import cartReducer from './cartSlice';

export const store = configureStore({
  reducer: {
    cart: cartReducer,
  },
});

// App.js
import { Provider } from 'react-redux';
import { store } from './store';

const App = () => (
  <Provider store={store}>
    <NavigationContainer>
      {/* Your navigation */}
    </NavigationContainer>
  </Provider>
);
```

### 8.2.3 Electron (Desktop)

**Structure d'une app Electron:**

```javascript
// main.js (Process principal)
const { app, BrowserWindow, ipcMain } = require('electron');
const path = require('path');

let mainWindow;

function createWindow() {
  mainWindow = new BrowserWindow({
    width: 1200,
    height: 800,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js'),
      contextIsolation: true,
      nodeIntegration: false,
    },
  });

  // Charger l'app (dev ou production)
  if (process.env.NODE_ENV === 'development') {
    mainWindow.loadURL('http://localhost:3000');
    mainWindow.webContents.openDevTools();
  } else {
    mainWindow.loadFile(path.join(__dirname, '../build/index.html'));
  }

  mainWindow.on('closed', () => {
    mainWindow = null;
  });
}

app.on('ready', createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow();
  }
});

// IPC Communication
ipcMain.handle('read-file', async (event, filePath) => {
  const fs = require('fs').promises;
  try {
    const content = await fs.readFile(filePath, 'utf-8');
    return { success: true, content };
  } catch (error) {
    return { success: false, error: error.message };
  }
});

ipcMain.handle('save-file', async (event, filePath, content) => {
  const fs = require('fs').promises;
  try {
    await fs.writeFile(filePath, content, 'utf-8');
    return { success: true };
  } catch (error) {
    return { success: false, error: error.message };
  }
});

// preload.js (Bridge sécurisé)
const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('electronAPI', {
  readFile: (filePath) => ipcRenderer.invoke('read-file', filePath),
  saveFile: (filePath, content) => ipcRenderer.invoke('save-file', filePath, content),
});

// renderer.js (Process de rendu - React/Vue/etc.)
import React, { useState } from 'react';

function App() {
  const [content, setContent] = useState('');

  const handleOpenFile = async () => {
    const result = await window.electronAPI.readFile('/path/to/file.txt');
    if (result.success) {
      setContent(result.content);
    } else {
      console.error(result.error);
    }
  };

  const handleSaveFile = async () => {
    const result = await window.electronAPI.saveFile('/path/to/file.txt', content);
    if (result.success) {
      alert('File saved!');
    } else {
      console.error(result.error);
    }
  };

  return (
    <div>
      <button onClick={handleOpenFile}>Open File</button>
      <textarea value={content} onChange={(e) => setContent(e.target.value)} />
      <button onClick={handleSaveFile}>Save File</button>
    </div>
  );
}
```

### 8.2.4 Qt (C++)

```cpp
// main.cpp
#include <QApplication>
#include "mainwindow.h"

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    
    MainWindow window;
    window.show();
    
    return app.exec();
}

// mainwindow.h
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QPushButton>
#include <QListWidget>
#include <QLineEdit>
#include <QVBoxLayout>

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private slots:
    void onAddButtonClicked();
    void onItemDoubleClicked(QListWidgetItem *item);

private:
    QLineEdit *inputField;
    QPushButton *addButton;
    QListWidget *listWidget;
    
    void setupUI();
    void loadProducts();
};

#endif

// mainwindow.cpp
#include "mainwindow.h"
#include <QVBoxLayout>
#include <QHBoxLayout>
#include <QLabel>
#include <QMessageBox>
#include <QNetworkAccessManager>
#include <QNetworkReply>
#include <QJsonDocument>
#include <QJsonArray>
#include <QJsonObject>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{
    setupUI();
    loadProducts();
}

void MainWindow::setupUI()
{
    // Widget central
    QWidget *centralWidget = new QWidget(this);
    setCentralWidget(centralWidget);

    // Layout principal
    QVBoxLayout *mainLayout = new QVBoxLayout(centralWidget);

    // Titre
    QLabel *titleLabel = new QLabel("Product Manager", this);
    QFont titleFont = titleLabel->font();
    titleFont.setPointSize(16);
    titleFont.setBold(true);
    titleLabel->setFont(titleFont);
    mainLayout->addWidget(titleLabel);

    // Input et bouton
    QHBoxLayout *inputLayout = new QHBoxLayout();
    inputField = new QLineEdit(this);
    inputField->setPlaceholderText("Enter product name...");
    addButton = new QPushButton("Add Product", this);
    connect(addButton, &QPushButton::clicked, this, &MainWindow::onAddButtonClicked);

    inputLayout->addWidget(inputField);
    inputLayout->addWidget(addButton);
    mainLayout->addLayout(inputLayout);

    // Liste
    listWidget = new QListWidget(this);
    connect(listWidget, &QListWidget::itemDoubleClicked, 
            this, &MainWindow::onItemDoubleClicked);
    mainLayout->addWidget(listWidget);

    // Taille fenêtre
    resize(600, 400);
    setWindowTitle("Qt Product Manager");
}

void MainWindow::loadProducts()
{
    QNetworkAccessManager *manager = new QNetworkAccessManager(this);
    
    QObject::connect(manager, &QNetworkAccessManager::finished,
        this, [this](QNetworkReply *reply) {
            if (reply->error() == QNetworkReply::NoError) {
                QByteArray data = reply->readAll();
                QJsonDocument doc = QJsonDocument::fromJson(data);
                QJsonArray products = doc.array();
                
                for (const QJsonValue &value : products) {
                    QJsonObject obj = value.toObject();
                    QString name = obj["name"].toString();
                    double price = obj["price"].toDouble();
                    
                    QString itemText = QString("%1 - $%2")
                        .arg(name)
                        .arg(price, 0, 'f', 2);
                    
                    listWidget->addItem(itemText);
                }
            }
            reply->deleteLater();
        });
    
    QNetworkRequest request(QUrl("https://api.example.com/products"));
    manager->get(request);
}

void MainWindow::onAddButtonClicked()
{
    QString text = inputField->text().trimmed();
    if (!text.isEmpty()) {
        listWidget->addItem(text);
        inputField->clear();
    }
}

void MainWindow::onItemDoubleClicked(QListWidgetItem *item)
{
    QMessageBox::information(this, "Product Details", 
                            "Selected: " + item->text());
}
```

---

## 8.3 Gestion des performances et ressources

### 8.3.1 Optimisation des performances

**React Native:**

```javascript
// Utiliser React.memo pour éviter re-renders inutiles
const ProductCard = React.memo(({ product, onPress }) => {
  return (
    <TouchableOpacity onPress={() => onPress(product)}>
      <View style={styles.card}>
        <Image source={{ uri: product.imageUrl }} style={styles.image} />
        <Text>{product.name}</Text>
      </View>
    </TouchableOpacity>
  );
}, (prevProps, nextProps) => {
  // Retourner true si les props sont égales (pas de re-render)
  return prevProps.product.id === nextProps.product.id;
});

// Utiliser FlatList avec optimisations
<FlatList
  data={products}
  renderItem={renderProduct}
  keyExtractor={(item) => item.id.toString()}
  // Optimisations
  initialNumToRender={10}
  maxToRenderPerBatch={10}
  windowSize={21}
  removeClippedSubviews={true}
  getItemLayout={(data, index) => ({
    length: 100,
    offset: 100 * index,
    index,
  })}
/>

// Lazy loading des images
import FastImage from 'react-native-fast-image';

<FastImage
  source={{
    uri: product.imageUrl,
    priority: FastImage.priority.normal,
    cache: FastImage.cacheControl.immutable,
  }}
  style={styles.image}
  resizeMode={FastImage.resizeMode.cover}
/>
```

**Flutter:**

```dart
// Utiliser const constructors
class ProductCard extends StatelessWidget {
  final Product product;
  
  const ProductCard({Key? key, required this.product}) : super(key: key);
  // ...
}

// ListView.builder pour lazy loading
ListView.builder(
  itemCount: products.length,
  itemBuilder: (context, index) {
    return ProductCard(product: products[index]);
  },
)

// Cache images
CachedNetworkImage(
  imageUrl: product.imageUrl,
  placeholder: (context, url) => CircularProgressIndicator(),
  errorWidget: (context, url, error) => Icon(Icons.error),
  memCacheWidth: 300,
  maxWidthDiskCache: 300,
)

// Éviter rebuild inutiles avec keys
ListView.builder(
  key: PageStorageKey('product-list'),
  // ...
)
```

### 8.3.2 Gestion de la mémoire

```javascript
// React Native - Cleanup dans useEffect
useEffect(() => {
  const subscription = api.subscribe(data => {
    setData(data);
  });

  // Cleanup
  return () => {
    subscription.unsubscribe();
  };
}, []);

// Libérer les ressources
useEffect(() => {
  return () => {
    // Annuler les requêtes en cours
    controller.abort();
    // Libérer les listeners
    listener.remove();
  };
}, []);
```

---

## 8.4 Connexion aux services cloud et aux API

### 8.4.1 Requêtes HTTP

**Flutter (http package):**

```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

class ApiService {
  static const String baseUrl = 'https://api.example.com';
  static const String apiKey = 'your-api-key';

  static Future<List<Product>> getProducts() async {
    try {
      final response = await http.get(
        Uri.parse('$baseUrl/products'),
        headers: {
          'Authorization': 'Bearer $apiKey',
          'Content-Type': 'application/json',
        },
      ).timeout(Duration(seconds: 10));

      if (response.statusCode == 200) {
        List<dynamic> data = json.decode(response.body);
        return data.map((json) => Product.fromJson(json)).toList();
      } else {
        throw Exception('Failed to load products: ${response.statusCode}');
      }
    } on TimeoutException {
      throw Exception('Request timeout');
    } catch (e) {
      throw Exception('Network error: $e');
    }
  }

  static Future<Product> createProduct(Product product) async {
    final response = await http.post(
      Uri.parse('$baseUrl/products'),
      headers: {
        'Authorization': 'Bearer $apiKey',
        'Content-Type': 'application/json',
      },
      body: json.encode(product.toJson()),
    );

    if (response.statusCode == 201) {
      return Product.fromJson(json.decode(response.body));
    } else {
      throw Exception('Failed to create product');
    }
  }
}
```

**React Native (Axios):**

```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Intercepteur pour ajouter le token
api.interceptors.request.use(
  async (config) => {
    const token = await AsyncStorage.getItem('authToken');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Intercepteur pour gérer les erreurs
api.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      // Token expiré, refresh ou logout
      await handleLogout();
    }
    return Promise.reject(error);
  }
);

export const ApiService = {
  getProducts: async () => {
    try {
      const response = await api.get('/products');
      return response.data;
    } catch (error) {
      throw new Error(error.response?.data?.message || 'Network error');
    }
  },

  createProduct: async (product) => {
    const response = await api.post('/products', product);
    return response.data;
  },
};
```

### 8.4.2 Stockage local

**Flutter (shared_preferences):**

```dart
import 'package:shared_preferences/shared_preferences.dart';

class StorageService {
  static Future<void> saveToken(String token) async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setString('auth_token', token);
  }

  static Future<String?> getToken() async {
    final prefs = await SharedPreferences.getInstance();
    return prefs.getString('auth_token');
  }

  static Future<void> saveUser(User user) async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setString('user', json.encode(user.toJson()));
  }

  static Future<User?> getUser() async {
    final prefs = await SharedPreferences.getInstance();
    final userJson = prefs.getString('user');
    if (userJson != null) {
      return User.fromJson(json.decode(userJson));
    }
    return null;
  }
}
```

**React Native (AsyncStorage):**

```javascript
import AsyncStorage from '@react-native-async-storage/async-storage';

export const StorageService = {
  saveToken: async (token) => {
    await AsyncStorage.setItem('auth_token', token);
  },

  getToken: async () => {
    return await AsyncStorage.getItem('auth_token');
  },

  saveUser: async (user) => {
    await AsyncStorage.setItem('user', JSON.stringify(user));
  },

  getUser: async () => {
    const userJson = await AsyncStorage.getItem('user');
    return userJson ? JSON.parse(userJson) : null;
  },

  clear: async () => {
    await AsyncStorage.clear();
  },
};
```

---

## 📝 Résumé du chapitre

✅ **Native vs Multiplateforme:** Trade-offs entre performance et time-to-market

✅ **Flutter:** Framework Google avec widgets et Dart, excellent pour UI

✅ **React Native:** JavaScript/React, large communauté, écosystème riche

✅ **Electron:** Desktop avec technologies web

✅ **Qt:** C++ pour desktop haute performance

✅ **Performances:** Optimisations critiques (lazy loading, memoization, caching)

✅ **APIs:** Gestion HTTP, authentification, stockage local

---

## 💡 Exercices pratiques

### Exercice 1
Créer une app mobile (Flutter ou React Native) de liste de tâches avec stockage local et synchronisation API.

### Exercice 2
Développer une application desktop (Electron ou Qt) pour gérer des fichiers avec preview.

### Exercice 3
Implémenter un système de cache efficace pour images et données API.

---

## 📚 Pour aller plus loin

- 📖 **"Flutter in Action"** - Eric Windmill
- 📖 **"React Native in Action"** - Nader Dabit
- 🔗 **flutter.dev**: Documentation officielle Flutter
- 🔗 **reactnative.dev**: Documentation React Native

---

[⬅️ Chapitre 7](./chapitre-7-developpement-web-api.md) | [➡️ Chapitre 9](./chapitre-9-bases-de-donnees-persistance.md)

