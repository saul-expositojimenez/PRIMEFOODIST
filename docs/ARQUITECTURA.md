# Arquitectura de PRIMEFOODIST

---

## Patrón arquitectónico

PRIMEFOODIST sigue el patrón **MVVM ligero** propio de SwiftUI:

- Las **Vistas** (`View`) describen la interfaz de forma declarativa.
- El **estado local** se gestiona con `@State` y `@Binding` dentro de las propias vistas.
- La **lógica de negocio** compartida (p. ej. geolocalización) se encapsula en clases `ObservableObject` con propiedades `@Published` (p. ej. `LocationManager`).
- **Firebase Auth** actúa como capa de servicio de autenticación, llamado directamente desde las vistas de login/registro.

No existe una capa ViewModel explícita para cada pantalla; el estado se mantiene lo más cerca posible de la vista que lo consume.

---

## Punto de entrada

```
PRIMEFOODISTApp  →  AppDelegate (configura Firebase)  →  ContentView
```

| Archivo | Responsabilidad |
|---|---|
| `PRIMEFOODISTApp.swift` | Define el `@main` de la app y registra el `AppDelegate` |
| `AppDelegate` (en el mismo archivo) | Llama a `FirebaseApp.configure()` al arrancar |
| `ContentView.swift` | Primera pantalla: imagen de fondo + botón "Empieza aquí" |

---

## Flujo de navegación

La navegación se basa en `NavigationStack` con `navigationDestination(isPresented:)` y `NavigationLink`. El diagrama de alto nivel es el siguiente:

```
ContentView
    └── OnboardingView (paso 1)
            └── OnboardingView2 (paso 2)
                    └── OnboardingView3 (paso 3)
                            └── LoginView
                                    ├── ForgotPasswordView
                                    │       └── PasswordResetConfirmationView
                                    ├── RegisterView (paso 1: edad/sexo)
                                    │       └── RegisterFormView (paso 2: datos de cuenta)
                                    │               └── UserTypeSelectionView
                                    │                       └── WelcomeCompletion / FinalCompletionView
                                    └── HomeView  ◄──────────────────────────────────────────────┐
                                            ├── DetalleEstablecimientoView                        │
                                            │       ├── ReseniasView                              │
                                            │       │       └── AniadirReseniaView                │
                                            │       └── GalletasView                              │
                                            ├── MapaView  ──────────────────────────────────────►┘
                                            ├── Categorías (Burgers / Cafeterías / Kebabs / Bares / Halal)
                                            │
                                            BottomNavigationBar (barra inferior fija)
                                                    ├──  HomeView
                                                    ├──  CalendarioView → ReservaView → Reserva2View → Reserva3View
                                                    ├──  CarritoView → Carrito2View → Carrito3View
                                                    └──  PerfilView
                                                                ├── EditarPerfilView
                                                                └── AjustesView
                                                                        ├── CuentaView
                                                                        │       └── EliminarCuentaView
                                                                        ├── AccesibilidadView
                                                                        ├── FAQView
                                                                        ├── PromocionesView
                                                                        ├── HistorialPedidosView
                                                                        └── HistorialReservasView
```

---

## Módulos del proyecto

### INTRO
Flujo de onboarding de bienvenida. Presenta 3 pantallas informativas antes de redirigir al usuario a `LoginView`. La navegación entre ellas usa `@State private var navigateToPageX = false` con `navigationDestination`.

### LOGIN
Gestiona la autenticación mediante **Firebase Auth**:
- `LoginView`: valida el correo y contraseña antes de llamar a `Auth.auth().signIn`.
- `ForgotPasswordView`: envía un correo de restablecimiento de contraseña.
- Incluye el componente reutilizable `ErrorNotificationView` para mostrar errores en forma de banner.

### REGISTRO
Flujo multi-paso para registrar nuevos usuarios:
1. `RegisterView` — recoge edad y sexo.
2. `RegisterFormView` — recoge nombre, correo, contraseña y confirma el registro con Firebase Auth.
3. `UserTypeSelectionView` — permite elegir el tipo de perfil.
4. `WelcomeCompletion` / `FinalCompletionView` — pantallas de confirmación.

Incluye el componente `RFNotificationBanner` con soporte para cuatro tipos de notificación (error, éxito, advertencia, info).

### HOME
Pantalla central de la aplicación:
- **Tendencias**: scroll horizontal de tarjetas `TrendingCard`.
- **Categorías**: botones de filtro + iconos emoji con animación (`CategoryIcon`).
- **Establecimientos**: lista vertical de tarjetas `EstablishmentCard`.
- Navegación a `DetalleEstablecimientoView` y `MapaView`.

### CATEGORÍAS
Cinco vistas independientes (una por categoría) que muestran los establecimientos filtrados por tipo:

| Vista | Categoría |
|---|---|
| `BurgersView` |  Hamburgueserías |
| `CafeteriasView` |  Cafeterías |
| `KebabsView` |  Kebabs |
| `BaresView` |  Bares |
| `HalalView` |  Comida Halal |

### CARRITO
Flujo de pedido en 3 pasos: selección de productos (`CarritoView`) → resumen (`Carrito2View`) → confirmación y pago (`Carrito3View`).

### CALENDARIO / RESERVAS
Flujo de reserva en 3 pasos: selección de fecha (`CalendarioView`) → detalles de la reserva (`ReservaView`, `Reserva2View`) → confirmación (`Reserva3View`).

### PERFIL
Vista principal del usuario con tres pestañas:
- **Reviews**: tarjetas de reseñas escritas por el usuario.
- **Favoritos**: establecimientos guardados como favoritos.
- **Siguiendo**: lista de perfiles seguidos.

Accede a `AjustesView` desde el icono de engranaje, que agrupa todas las opciones de configuración de cuenta.

### PERMISOUBICACION
- `LocationManager`: clase `ObservableObject` que envuelve `CLLocationManager`. Solicita el permiso `whenInUse`, obtiene la coordenada actual y la geocodifica en un nombre de lugar legible.
- `LocationSelectionView`, `AddLocationView`, `ChangeLocationView`: vistas para seleccionar y cambiar la dirección de entrega.

---

## Gestión del estado

| Mecanismo | Uso |
|---|---|
| `@State` | Estado local de una vista (campos de formulario, flags de navegación, animaciones) |
| `@Binding` | Pasar estado editable al hijo (p. ej. `selectedTab` en `BottomNavigationBar`) |
| `@Environment(\.dismiss)` | Cerrar la vista actual sin NavigationLink |
| `@Published` + `ObservableObject` | Estado compartido entre vistas (p. ej. `LocationManager`) |

---

## Dependencias externas

| Librería | Uso |
|---|---|
| `FirebaseCore` | Inicialización de Firebase al arrancar la app |
| `FirebaseAuth` | Autenticación de usuarios (registro, login, recuperación de contraseña) |
| `MapKit` | Mapa interactivo en `MapaView` |
| `CoreLocation` | Obtención de coordenadas GPS y geocodificación inversa |
| `Combine` | Publicación reactiva de la ubicación del usuario desde `LocationManager` |
