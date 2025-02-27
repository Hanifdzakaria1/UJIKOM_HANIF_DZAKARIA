# Fullstack Database Project

## Deskripsi Proyek
Proyek ini menggunakan **Sequelize** sebagai ORM untuk mengelola database. Proyek ini mencakup berbagai model data seperti **User, Product, Ulasan, dan Merek** serta menggunakan **request.test** untuk melakukan pengujian API. Sistem ini juga mendukung routing di frontend dan backend untuk mengatur alur data.

---

## Instalasi
### Prerequisites
Pastikan Anda telah menginstal:
- **Node.js**
- **Sequelize CLI**
- **MySQL atau PostgreSQL** (sesuai dengan kebutuhan proyek)

## note penting : 
- nodemoduls saya hapus karna agar tidak memberatkan laptop saya
- saya mengkompres menjadi ZIP file agar lebih ringan
- sebelum mengisntal harap di perhatikan dan di kompres terlebih dahulu 

### Langkah Instalasi
1. Clone repository ini:
   ```bash
   git clone https://github.com/username/project-name.git
   cd project-name
   ```
2. Instal dependensi:
   ```bash
   npm install
   ```
3. Konfigurasi database:
   - Atur file **.env** untuk koneksi database
   ```env
   DB_NAME=your_database_name
   DB_USER=your_database_user
   DB_PASS=your_database_password
   DB_HOST=localhost
   DB_DIALECT=mysql  # atau postgres
   ```
4. Jalankan migrasi database:
   ```bash
   npx sequelize db:migrate
   ```
5. Jalankan aplikasi:
   ```bash
   npm start
   ```

---

## Model-Model Database
Berikut adalah model-model yang digunakan dalam proyek ini:

### 1. UserModel
Digunakan untuk menyimpan informasi pengguna.
```js
const { DataTypes } = require('sequelize');
const sequelize = require('../config/database');

const User = sequelize.define('User', {
  id: { type: DataTypes.INTEGER, primaryKey: true, autoIncrement: true },
  username: { type: DataTypes.STRING, allowNull: false },
  email: { type: DataTypes.STRING, allowNull: false, unique: true },
  password: { type: DataTypes.STRING, allowNull: false }
});

module.exports = User;
```

### 2. ProductModel
Merepresentasikan data produk.
```js
const Product = sequelize.define('Product', {
  id: { type: DataTypes.INTEGER, primaryKey: true, autoIncrement: true },
  name: { type: DataTypes.STRING, allowNull: false },
  price: { type: DataTypes.FLOAT, allowNull: false },
  stock: { type: DataTypes.INTEGER, allowNull: false }
});
```

### 3. UlasanModel
Menyimpan ulasan produk oleh pengguna.
```js
const Ulasan = sequelize.define('Ulasan', {
  id: { type: DataTypes.INTEGER, primaryKey: true, autoIncrement: true },
  userId: { type: DataTypes.INTEGER, allowNull: false },
  productId: { type: DataTypes.INTEGER, allowNull: false },
  rating: { type: DataTypes.INTEGER, allowNull: false },
  comment: { type: DataTypes.TEXT, allowNull: true }
});
```

### 4. MerekModel
Menyimpan informasi merek produk.
```js
const Merek = sequelize.define('Merek', {
  id: { type: DataTypes.INTEGER, primaryKey: true, autoIncrement: true },
  name: { type: DataTypes.STRING, allowNull: false }
});
```

---

## Controller
Controller digunakan untuk menangani logika aplikasi. Berikut adalah beberapa controller utama:

### 1. UserController
```js
const User = require('../models/UserModel');
exports.getAllUsers = async (req, res) => {
  const users = await User.findAll();
  res.json(users);
};
```

### 2. ProductController
```js
const Product = require('../models/ProductModel');
exports.getAllProducts = async (req, res) => {
  const products = await Product.findAll();
  res.json(products);
};
```

### 3. UlasanController
```js
const Ulasan = require('../models/UlasanModel');
exports.getAllReviews = async (req, res) => {
  const reviews = await Ulasan.findAll();
  res.json(reviews);
};
```

### 4. MerekController
```js
const Merek = require('../models/MerekModel');
exports.getAllBrands = async (req, res) => {
  const brands = await Merek.findAll();
  res.json(brands);
};
```

---

## Routing
Routing digunakan untuk menghubungkan frontend dengan backend.

### Backend Routes
Routes backend didefinisikan dalam **routes.js**.
```js
const express = require('express');
const router = express.Router();
const UserController = require('../controllers/UserController');
const ProductController = require('../controllers/ProductController');
const UlasanController = require('../controllers/UlasanController');
const MerekController = require('../controllers/MerekController');

// User Routes
router.get('/users', UserController.getAllUsers);

// Product Routes
router.get('/products', ProductController.getAllProducts);

// Ulasan Routes
router.get('/reviews', UlasanController.getAllReviews);

// Merek Routes
router.get('/brands', MerekController.getAllBrands);

module.exports = router;
```

### Frontend Routing
Frontend menggunakan framework seperti **React.js** dengan contoh sistem routing berikut:

```js
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import Home from './pages/Home';
import Products from './pages/Products';
import Reviews from './pages/Reviews';
import Brands from './pages/Brands';

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/products" element={<Products />} />
        <Route path="/reviews" element={<Reviews />} />
        <Route path="/brands" element={<Brands />} />
      </Routes>
    </Router>
  );
}
export default App;
```

---

## Testing dengan request.test
Untuk pengujian, gunakan **request.test** untuk memastikan API berjalan dengan baik.

Contoh pengujian:
```js
const request = require('supertest');
const app = require('../app');

test('GET /users should return users', async () => {
  const res = await request(app).get('/users');
  expect(res.status).toBe(200);
  expect(Array.isArray(res.body)).toBeTruthy();
});
```

---

## Kesimpulan
Proyek ini menggunakan **Sequelize** untuk mengelola database dengan model **User, Product, Ulasan, dan Merek**. API dibuat menggunakan **Express.js** dan diuji menggunakan **request.test**. Routing digunakan di backend untuk menyediakan data ke frontend yang dapat menggunakan React atau Vue.

---

## By Hanif Dzakaria
