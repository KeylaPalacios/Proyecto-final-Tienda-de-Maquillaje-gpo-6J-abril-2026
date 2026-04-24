Esta es una estructura robusta para implementar tu visión de **agente global `.agents`**. Vamos a construir el ecosistema desde la definición de la habilidad hasta el código base del proyecto "proyectotiendademaquillaje".

---

## 1. Estructura de la Habilidad Agente Global `.agents`

Primero, definimos el cerebro de tu automatización. Esta carpeta servirá como la "documentación maestra" para tus agentes de diseño, código y scraping.

### Carpeta: `.agents/`
* **SKILL.md**: Define las capacidades y flujos de trabajo.
* **scripts/**: Scripts de automatización (bash/dart) para tareas repetitivas.
* **ejemplos/**: Snippets de código de referencia.
* **resources/**: Activos, iconos y esquemas de base de datos.

#### Contenido de `.agents/SKILL.md`
```markdown
# Skill: Agente Global de Automatización de Tiendas
Este agente gestiona el ciclo de vida de apps Flutter con Firebase.

## Sub-Skills
1. **Diseño**: Generación de UI limpia para eCommerce.
2. **Código**: Implementación de lógica CRUD y estados.
3. **Scraping**: Extracción de catálogos de maquillaje (opcional/futuro).

## Flujo de Trabajo
- Verificar entorno -> Vincular Firebase -> Generar Estructura -> Desplegar CRUD.
```

---

## 2. Prerrequisitos y Configuración del Entorno

Antes de tocar el código, aseguremos que tus "motores" estén encendidos.

### Verificación de Herramientas
Ejecuta esto en tu terminal (VS Code / Antigravity):
```bash
# Verificar Flutter
flutter --version

# Instalar FlutterFire CLI (si no lo tienes)
dart pub global activate flutterfire_cli

# Login en Firebase
firebase login
```

### Configuración del Proyecto
1.  **Consola Firebase**: Crea un proyecto llamado `maquillaje-tienda`.
2.  **Firestore**: Habilita la base de datos en "Modo de prueba" (para desarrollo).
3.  **Vínculo**: Desde la raíz de tu carpeta `proyectotiendademaquillaje`, ejecuta:
    ```bash
    flutterfire configure
    ```

---

## 3. Estructura del Proyecto: `proyectotiendademaquillaje`



### Configuración `pubspec.yaml`
Añade las dependencias necesarias:
```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.0.0
  cloud_firestore: ^4.0.0
  cupertino_icons: ^1.0.2
```

---

## 4. Implementación del Código (Dart/Flutter)

### A. Modelo de Producto (`lib/models/product_model.dart`)
```dart
class Product {
  String id;
  String nombre;
  double precio;
  String imagenUrl;

  Product({required this.id, required this.nombre, required this.precio, required this.imagenUrl});

  Map<String, dynamic> toMap() => {
    'nombre': nombre,
    'precio': precio,
    'imagenUrl': imagenUrl,
  };

  factory Product.fromMap(String id, Map<String, dynamic> map) => Product(
    id: id,
    nombre: map['nombre'] ?? '',
    precio: (map['precio'] ?? 0).toDouble(),
    imagenUrl: map['imagenUrl'] ?? '',
  );
}
```

### B. Servicio Firestore (`lib/services/firebase_service.dart`)
```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/product_model.dart';

class FirebaseService {
  final CollectionReference _db = FirebaseFirestore.instance.collection('productos');

  Stream<List<Product>> getProducts() {
    return _db.snapshots().map((snap) =>
        snap.docs.map((doc) => Product.fromMap(doc.id, doc.data() as Map<String, dynamic>)).toList());
  }

  Future<void> addProduct(Product p) => _db.add(p.toMap());
  Future<void> updateProduct(Product p) => _db.doc(p.id).update(p.toMap());
  Future<void> deleteProduct(String id) => _db.doc(id).delete();
}
```

### C. UI: Pantalla Principal y CRUD (`lib/screens/home_screen.dart`)
```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';
import '../models/product_model.dart';

class HomeScreen extends StatelessWidget {
  final FirebaseService _service = FirebaseService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Tienda de Maquillaje ✨")),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () => _showForm(context),
      ),
      body: StreamBuilder<List<Product>>(
        stream: _service.getProducts(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          final products = snapshot.data!;
          return ListView.builder(
            itemCount: products.length,
            itemBuilder: (context, i) => ListTile(
              title: Text(products[i].nombre),
              subtitle: Text("\$${products[i].precio}"),
              trailing: Row(
                mainAxisSize: MainAxisSize.min,
                children: [
                  IconButton(icon: Icon(Icons.edit), onPressed: () => _showForm(context, products[i])),
                  IconButton(icon: Icon(Icons.delete), onPressed: () => _service.deleteProduct(products[i].id)),
                ],
              ),
            ),
          );
        },
      ),
    );
  }

  void _showForm(BuildContext context, [Product? product]) {
    final nameCtrl = TextEditingController(text: product?.nombre);
    final priceCtrl = TextEditingController(text: product?.precio.toString());

    showModalBottomSheet(
      context: context,
      isScrollControlled: true,
      builder: (context) => Padding(
        padding: EdgeInsets.only(bottom: MediaQuery.of(context).viewInsets.bottom, left: 20, right: 20, top: 20),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(controller: nameCtrl, decoration: InputDecoration(labelText: "Producto")),
            TextField(controller: priceCtrl, decoration: InputDecoration(labelText: "Precio"), keyboardType: TextInputType.number),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                final p = Product(
                  id: product?.id ?? '',
                  nombre: nameCtrl.text,
                  precio: double.parse(priceCtrl.text),
                  imagenUrl: '',
                );
                product == null ? _service.addProduct(p) : _service.updateProduct(p);
                Navigator.pop(context);
              },
              child: Text(product == null ? "Crear" : "Actualizar"),
            )
          ],
        ),
      ),
    );
  }
}
```

---

## 5. Inicialización Principal (`lib/main.dart`)

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'firebase_options.dart'; // Generado por flutterfire configure
import 'screens/home_screen.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform);
  runApp(MaquillajeApp());
}

class MaquillajeApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(primarySwatch: Colors.pink),
      home: HomeScreen(),
    );
  }
}
```

---

## Resumen de carpetas generadas:
1.  `.agents/`: Inteligencia de automatización.
2.  `lib/models/`: Estructura de datos.
3.  `lib/services/`: Lógica de Firestore.
4.  `lib/screens/`: Interfaz de usuario (UI).

Este proyecto ya es **totalmente funcional**. Al ejecutar `flutter run`, tendrás una app conectada a la nube donde puedes gestionar tu inventario de maquillaje en tiempo real.
