# Proyecto Flutter - Arquitectura Feature First

Este proyecto sigue una arquitectura **Feature First** orientada a escalabilidad y mantenibilidad.

El objetivo es que la aplicación pueda comenzar siendo pequeña y crecer sin que el código se vuelva difícil de mantener.

---

# Tecnologías

- Flutter
- Dart
- Material Design
- SharedPreferences (persistencia local)
- API REST (futuro)
- Riverpod o Bloc (futuro)

---

# Estructura de carpetas

```text
lib/
│
├── core/
│   ├── routes/
│   ├── theme/
│   ├── storage/
│   ├── services/
│   └── widgets/
│
├── features/
│
│   ├── onboarding/
│   │
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   ├── models/
│   │   │   └── repositories/
│   │   │
│   │   ├── domain/
│   │   │   ├── repositories/
│   │   │   └── usecases/
│   │   │
│   │   └── presentation/
│   │       ├── controllers/
│   │       ├── pages/
│   │       └── widgets/
│   │
│   └── home/
│       │
│       ├── data/
│       ├── domain/
│       └── presentation/
│
└── main.dart
```

---

# Explicación de cada carpeta

## core/

Contiene funcionalidades compartidas por toda la aplicación.

Ejemplos:

```text
core/
├── routes/
├── theme/
├── storage/
├── services/
└── widgets/
```

### routes/

Configuración de rutas globales.

Ejemplo:

```dart
AppRoutes
```

---

### theme/

Configuración visual de la aplicación.

Ejemplo:

```dart
AppTheme
```

---

### storage/

Acceso a almacenamiento local.

Ejemplo:

```dart
SharedPreferences
Hive
SecureStorage
```

---

### services/

Servicios reutilizables.

Ejemplo:

```dart
HttpClient
ApiService
LoggerService
```

---

### widgets/

Widgets reutilizables globalmente.

Ejemplo:

```dart
PrimaryButton
LoadingWidget
CustomTextField
```

---

# features/

Cada funcionalidad de la aplicación se encuentra aislada dentro de una feature.

Ejemplos:

```text
features/
├── onboarding/
├── home/
├── auth/
├── profile/
└── cars/
```

---

# Arquitectura interna de una feature

Cada feature sigue la siguiente estructura:

```text
feature/
├── data/
├── domain/
└── presentation/
```

---

# presentation/

Contiene la interfaz gráfica y el manejo de estado.

```text
presentation/
├── controllers/
├── pages/
└── widgets/
```

---

## pages/

Pantallas completas.

Ejemplo:

```dart
OnboardingPage
HomePage
LoginPage
```

---

## widgets/

Componentes visuales reutilizables de la feature.

Ejemplo:

```dart
OnboardingStep
CarCard
LoginForm
```

---

## controllers/

Lógica de presentación.

Responsabilidades:

- Navegación
- Estado de la pantalla
- Manejo de formularios
- Comunicación con casos de uso

Ejemplo:

```dart
OnboardingController
HomeController
```

---

# domain/

Contiene las reglas del negocio.

No debe depender de Flutter.

```text
domain/
├── repositories/
└── usecases/
```

---

## repositories/

Contratos (interfaces).

Ejemplo:

```dart
abstract class CarsRepository
```

---

## usecases/

Casos de uso de la aplicación.

Ejemplos:

```dart
GetCars
CreateCar
CompleteOnboarding
```

---

# data/

Contiene la implementación real de acceso a datos.

```text
data/
├── datasources/
├── models/
└── repositories/
```

---

## datasources/

Origen de los datos.

Ejemplos:

```dart
ApiDatasource
LocalDatasource
FirebaseDatasource
```

---

## models/

Modelos que representan respuestas externas.

Ejemplo:

```dart
CarModel
UserModel
```

---

## repositories/

Implementaciones de los contratos definidos en domain.

Ejemplo:

```dart
CarsRepositoryImpl
```

---

# Flujo inicial de la aplicación

Al iniciar la aplicación:

```text
main.dart
    │
    ▼
Verificar onboarding
    │
 ┌──┴──┐
 │     │
 ▼     ▼
NO    SI
 │     │
 ▼     ▼
Onboarding Home
```

---

# Flujo del onboarding

```text
Paso 1
  │
  ▼
Paso 2
  │
  ▼
Guardar onboarding completado
  │
  ▼
Home
```

El onboarding debe mostrarse únicamente la primera vez que se instala la aplicación.

La información será almacenada localmente utilizando SharedPreferences.

---

# Flujo Home

```text
Home
 │
 ▼
Controller
 │
 ▼
UseCase
 │
 ▼
Repository
 │
 ▼
Datasource
 │
 ▼
API
```

---

# Ejemplo de crecimiento

Actualmente:

```text
features/
├── onboarding/
└── home/
```

Futuro:

```text
features/
├── onboarding/
├── home/
├── auth/
├── profile/
├── cars/
├── payments/
└── notifications/
```

La arquitectura permite agregar nuevas funcionalidades sin afectar las existentes.

---

# Instalación

## 1. Clonar repositorio

```bash
git clone <url-del-repositorio>
```

---

## 2. Ingresar al proyecto

```bash
cd nombre-del-proyecto
```

---

## 3. Instalar dependencias

```bash
flutter pub get
```

---

## 4. Verificar Flutter

```bash
flutter doctor
```

Corregir cualquier error mostrado por Flutter Doctor antes de continuar.

---

## 5. Ejecutar aplicación

```bash
flutter run
```

---

# Compilar APK

```bash
flutter build apk
```

APK generado:

```text
build/app/outputs/flutter-apk/app-release.apk
```

---

# Buenas prácticas

## Sí hacer

- Organizar por features.
- Mantener widgets pequeños.
- Mantener controllers enfocados.
- Utilizar repositorios para acceso a datos.
- Separar UI de lógica.

---

## Evitar

No utilizar estructuras como:

```text
screens/
widgets/
services/
models/
```

para toda la aplicación, ya que generan carpetas gigantes difíciles de mantener.

---

# Convenciones de nombres

## Pages

```dart
home_page.dart
login_page.dart
onboarding_page.dart
```

---

## Widgets

```dart
car_card.dart
primary_button.dart
onboarding_step.dart
```

---

## Controllers

```dart
home_controller.dart
auth_controller.dart
onboarding_controller.dart
```

---

## Repositories

```dart
cars_repository.dart
cars_repository_impl.dart
```

---

# Objetivo de la arquitectura

Permitir que la aplicación:

- Sea fácil de mantener.
- Sea fácil de escalar.
- Permita agregar nuevas funcionalidades rápidamente.
- Mantenga una separación clara entre UI, negocio y datos.
- Facilite pruebas unitarias y futuras integraciones.