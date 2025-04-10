# app_jic

# 🧠 Arquitectura del Proyecto - APPJIC

Este proyecto está basado en una estructura modular tipo **Clean Architecture + Feature-first**, ideal para mantener un código limpio, escalable y fácil de probar. Está orientado a una aplicación de aprendizaje motriz para niños con Síndrome de Down.

---

## 📂 Estructura general

```
lib/
├── core/                  # Código reutilizable y transversal
│   ├── errors/            # Manejo de errores globales
│   ├── utils/             # Funciones de utilidad
│   ├── widgets/           # Widgets reutilizables a nivel global
│   ├── theme/             # Estilos, colores, tipografías
│   └── services/          # Servicios generales (voz, movimiento, RPi)
│
├── features/              # Módulos de funcionalidades específicas
│   ├── activities/        # Lógica y UI de actividades educativas
│   ├── progress_tracking/ # Seguimiento del progreso del niño
│   ├── user_profile/      # Perfil del usuario (niño)
│   └── ...                # Otros módulos futuros
└── main.dart              # Punto de entrada de la app
```

---

## 📌 Descripción de carpetas clave

### `core/`

Contiene utilidades reutilizables a través de toda la app:

- `errors/`: Excepciones comunes como `ServerFailure`, `CacheException`, etc.
- `utils/`: Validadores, formateadores, conversiones de datos.
- `widgets/`: Widgets globales como botones, loaders, headers.
- `theme/`: Configuración de tema, colores y tipografías.
- `services/`: Interfaces con el hardware/software:
  - `voice_recognition_service.dart`
  - `motion_detection_service.dart`
  - `raspberry_connection.dart`

---

### `features/`

Cada carpeta representa una funcionalidad independiente y autocontenida.

#### `activities/`

- Representa las lecciones y ejercicios (colores, formas, lenguaje, etc.).
- Usa animaciones, botones físicos, reconocimiento de voz o movimiento.
- Conecta con `progress_tracking/` para registrar resultados.

#### `progress_tracking/`

- Lógica de seguimiento y estadísticas del progreso del niño.
- Contiene:
  - `domain/`: Casos de uso y entidades puras (`Progreso`, `EstadísticaMensual`)
  - `data/`: Modelos, fuentes de datos y repositorios
  - `presentation/`: UI de progreso y estadísticas
- Conecta con `activities/` para registrar resultados y con `user_profile/` para asociarlos al usuario.

#### `user_profile/`

- Información personal del niño (edad, nombre, necesidades).
- Personaliza actividades y agrupa métricas del progreso.

---

## 🔁 Conexiones entre módulos

```
main.dart
 ├─ Inicia rutas, dependencias y temas
 ├─ Usa core/theme/ para aplicar estilo
 └─ Usa core/services/ para funcionalidades como voz, movimiento o hardware

activities/
 └─ Llama a progress_tracking/ al completar actividades

progress_tracking/
 └─ Se alimenta desde activities/ y se conecta con user_profile/

user_profile/
 └─ Define el usuario actual y afecta las actividades presentadas
```

---

## 📊 Seguimiento del progreso

La app registrará:

- Resultados por actividad
- Aciertos y errores
- Progreso a lo largo del tiempo (día, semana, mes, año)

La lógica estará dentro de `features/progress_tracking/`. Puede integrarse con herramientas como:

- [Hive](https://pub.dev/packages/hive) o [Isar](https://isar.dev/) para almacenamiento local
- [Firebase Analytics](https://firebase.google.com/products/analytics)
- [Supabase](https://supabase.com/) para tracking remoto

---

## 🔌 Integraciones externas

Se planifica el uso de:

- 🎤 Reconocimiento de voz (e.g. `speech_to_text`)
- 📷 Reconocimiento de movimiento (e.g. `google_ml_kit` o `tflite`)
- 🔘 Botones físicos conectados a Raspberry Pi

Estas integraciones están organizadas dentro de `core/services/`.

---

> Esta estructura permite escalar el proyecto por módulos, facilitar las pruebas unitarias y aislar la lógica de negocio del UI.



A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.
