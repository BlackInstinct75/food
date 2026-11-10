# 🥗 NutriTrack (Personal Food Logger)

Un sistema de registro de comidas y seguimiento de macros desarrollado como proyecto personal y experimento técnico. El proyecto combina un backend rápido e hiper-eficiente en Rust, una aplicación móvil nativa en Kotlin (Android) y un frontend web experimental en C++ compilado a WebAssembly.

---

## 🛠️ Arquitectura y Stack Tecnológico

El objetivo del proyecto es mantener un control de alimentos consumidos sin complejidad innecesaria, priorizando el rendimiento, la exploración de tecnologías y el reto de ingeniería.

| Componente | Tecnología | Descripción |
| :--- | :--- | :--- |
| **Backend** | **Rust** (Axum + SQLx + SQLite) | API REST ligera y segura en memoria para la gestión de usuarios, alimentos y logs diarios. |
| **App Móvil** | **Kotlin** (Jetpack Compose) | Cliente Android nativo con UI declarativa para un registro rápido desde el teléfono. |
| **Frontend Web** | **C++** (Dear ImGui / Raylib + WebAssembly) | Interfaz web funcional renderizada en Canvas mediante C++ compilado a Wasm con Emscripten. |

```
+-------------------------------------------------------+
|                 Frontend Web (C++ / Wasm)             |
+---------------------------+---------------------------+
                            | (HTTP / JSON)
                            v
+-------------------------------------------------------+
|                  Backend API (Rust / Axum)            | <---> [ SQLite DB ]
+---------------------------+---------------------------+
                            ^
                            | (HTTP / JSON)
+---------------------------+---------------------------+
|               App Móvil (Kotlin / Compose)            |
+-------------------------------------------------------+
```

---

## 🚀 Estructura del Proyecto

```text
nutritrack/
├── backend/            # API Server escrito en Rust
│   ├── src/
│   │   ├── main.rs
│   │   ├── routes/
│   │   ├── models/
│   │   └── db/
│   └── Cargo.toml
├── mobile/             # Aplicación Android en Kotlin
│   ├── app/
│   │   └── src/main/java/com/nutritrack/
│   └── build.gradle.kts
└── web/                # Frontend Web en C++ (Wasm)
    ├── src/
    │   └── main.cpp
    ├── Makefile / CMakeLists.txt
    └── index.html
```

---

## 📊 Modelo de Datos Básicos

### Alimento (Food)
* `id`: UUID / Integer
* `name`: String
* `calories`: f32 (kcal / 100g)
* `protein`: f32 (g / 100g)
* `carbs`: f32 (g / 100g)
* `fat`: f32 (g / 100g)

### Registro (Log Entry)
* `id`: UUID / Integer
* `date`: NaiveDate (YYYY-MM-DD)
* `meal_type`: Enum (Desayuno, Almuerzo, Cena, Snack)
* `food_id`: ID de referencia del alimento
* `weight_grams`: f32 (cantidad consumida)

---

## ⚙️ Configuración y Compilación

### 1. Backend (Rust)

**Requisitos:** Rust (Toolchain estable), Cargo.

```bash
cd backend
cargo run
```
*El servidor se ejecutará en `http://localhost:3000`.*

### 2. Frontend Web (C++ a Wasm)

**Requisitos:** Compiler C++ (GCC/Clang), [Emscripten SDK (emsdk)](https://emscripten.org/).

```bash
# Activar el entorno de Emscripten
source ./emsdk_env.sh

# Compilar a WebAssembly
cd web
emcc src/main.cpp -o index.html -s USE_WEBGL2=1 -s WASM=1

# Servir el frontend localmente
python3 -m http.server 8080
```
*Abre `http://localhost:8080` en tu navegador.*

### 3. App Móvil (Kotlin)

**Requisitos:** Android Studio (Ladybug o superior), JDK 17+.

1. Abrir la carpeta `mobile/` en Android Studio.
2. Sincronizar las dependencias Gradle.
3. Ejecutar en emulador o dispositivo físico Android (asegurando apuntar la IP del backend a `10.0.2.2` para emulador).

---

## 📌 Próximos Pasos & Roadmap

- [ ] Implementar la API REST en Axum con almacenamiento SQLite.
- [ ] Construir la interfaz de usuario con Dear ImGui compilada en C++/Wasm.
- [ ] Desarrollar la pantalla de registro rápido en Android con Compose.
- [ ] Añadir sincronización/caché local en la app Android.