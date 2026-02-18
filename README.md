---
# 🛒 TUGAS PERTEMUAN 4 - PART 5  
## MINI SHOPPING CART APP  
### Mata Kuliah: Pemrograman Aplikasi Bergerak
---

### 👤 **Nama:** Muhammad Ilyasa' Izzuddin  
### 🎓 **Kelas:** A 2024 - Sistem Informasi  
### 🆔 **NIM:** 2409116033  

---

## 📌 Deskripsi Singkat

Aplikasi Mini E-Commerce sederhana menggunakan **Flutter** dengan 
state management **Provider (ChangeNotifier)**.

Aplikasi ini mampu:
- Menampilkan daftar produk
- Menambahkan produk ke keranjang
- Mengatur jumlah item
- Menghapus item
- Menghitung total harga otomatis
- Melakukan checkout

---

# 🧠 Konsep yang Digunakan

> 💡 **State Management dengan Provider**
>
> CartModel mewarisi `ChangeNotifier`  
> Setiap perubahan data akan memanggil `notifyListeners()`  
> UI akan otomatis ter-update menggunakan `Consumer`

---

# 📂 Struktur Project

```bash
lib/
 ├── models/
 │    ├── product.dart
 │    ├── cart_item.dart
 │    └── cart_model.dart
 │
 ├── pages/
 │    ├── product_list_page.dart
 │    └── cart_page.dart
 │
 └── main.dart
```

---

# 💻 SOURCE CODE

---

## 🧱 1️⃣ Product Model

📁 `lib/models/product.dart`

```dart
class Product {
  final String id;
  final String name;
  final double price;
  final String imageUrl;
  final String category;

  Product({
    required this.id,
    required this.name,
    required this.price,
    required this.imageUrl,
    required this.category,
  });
}
```

> ✨ Model ini digunakan untuk merepresentasikan data produk.

---

## 🛒 2️⃣ Cart Item Model

📁 `lib/models/cart_item.dart`

```dart
import 'product.dart';

class CartItem {
  final Product product;
  int quantity;
  
  CartItem({
    required this.product,
    this.quantity = 1,
  });
  
  double get totalPrice => product.price * quantity;
}
```

> ✨ Menyimpan produk + jumlahnya serta menghitung subtotal otomatis.

---

## 🧠 3️⃣ Cart Model (State Management)

📁 `lib/models/cart_model.dart`

```dart
class CartModel extends ChangeNotifier {
  final Map<String, CartItem> _items = {};

  Map<String, CartItem> get items => _items;
  List<CartItem> get itemsList => _items.values.toList();

  int get itemCount => _items.length;

  int get totalQuantity =>
      _items.values.fold(0, (sum, item) => sum + item.quantity);

  double get totalPrice =>
      _items.values.fold(0.0, (sum, item) => sum + item.totalPrice);

  bool get isEmpty => _items.isEmpty;

  void addItem(Product product) {
    if (_items.containsKey(product.id)) {
      _items[product.id]!.quantity++;
    } else {
      _items[product.id] = CartItem(product: product);
    }
    notifyListeners();
  }

  void removeItem(String productId) {
    _items.remove(productId);
    notifyListeners();
  }

  void increaseQuantity(String productId) {
    _items[productId]?.quantity++;
    notifyListeners();
  }

  void decreaseQuantity(String productId) {
    if (!_items.containsKey(productId)) return;

    if (_items[productId]!.quantity > 1) {
      _items[productId]!.quantity--;
    } else {
      _items.remove(productId);
    }

    notifyListeners();
  }

  void clear() {
    _items.clear();
    notifyListeners();
  }
}
```

> 💡 Menggunakan `Map<String, CartItem>` untuk pencarian cepat (O(1)).  
> Semua perubahan memanggil `notifyListeners()` agar UI otomatis update.

---

## 🚀 4️⃣ Main (Provider Setup)

📁 `main.dart`

```dart
void main() {
  runApp(
    ChangeNotifierProvider(
      create: (context) => CartModel(),
      child: const MyApp(),
    ),
  );
}
```

> ✨ CartModel dibungkus dengan `ChangeNotifierProvider` agar bisa diakses seluruh aplikasi.

---

## 🛍️ 5️⃣ Product List Page

📁 `lib/pages/product_list_page.dart`

Menampilkan daftar produk menggunakan `GridView` dan tombol **Add to Cart**.

> ✨ Menggunakan `context.read<CartModel>().addItem(product)`  
> ✨ Menampilkan badge jumlah item di icon keranjang  
> ✨ Menggunakan SnackBar sebagai feedback

---

## 🧾 6️⃣ Cart Page

📁 `lib/pages/cart_page.dart`

Fitur:
- Menampilkan item dalam ListView
- Increase / Decrease quantity
- Remove item
- Total harga otomatis
- Clear cart
- Checkout dialog

> ✨ Jika cart kosong akan menampilkan pesan "Your cart is empty"

---
---

# 📸 Tampilan Aplikasi

<div align="center">

| 🛍️ UI RESULT |
|------------------|
| <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/139928e7-8cf7-418f-ad39-90dd73419dab" />|

</div>

---

# 🎯 Kesimpulan

Aplikasi ini berhasil mengimplementasikan:

- State Management menggunakan Provider
- Penggunaan ChangeNotifier
- Perhitungan total harga menggunakan `fold()`
- UI otomatis update saat data berubah

Project ini melatih pemahaman tentang:
- Arsitektur model-view
- Pengelolaan state
- Interaksi UI dan data

---

⭐ Project ini dibuat sebagai latihan implementasi State Management pada Flutter.


