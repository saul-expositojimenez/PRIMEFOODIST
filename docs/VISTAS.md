# Vistas y componentes de PRIMEFOODIST

Este documento describe cada vista y componente reutilizable del proyecto.

---

## Índice

- [Vistas de inicio](#vistas-de-inicio)
- [Onboarding](#onboarding)
- [Autenticación](#autenticación)
- [Registro](#registro)
- [Home](#home)
- [Detalle de establecimiento](#detalle-de-establecimiento)
- [Mapa](#mapa)
- [Categorías](#categorías)
- [Carrito](#carrito)
- [Reservas / Calendario](#reservas--calendario)
- [Perfil](#perfil)
- [Ajustes y cuenta](#ajustes-y-cuenta)
- [Ubicación](#ubicación)
- [Componentes reutilizables](#componentes-reutilizables)

---

## Vistas de inicio

### `ContentView`
**Archivo:** `PRIMEFOODIST/ContentView.swift`

Vista raíz de la aplicación. Muestra una imagen de fondo a pantalla completa (`backgroundintro`) y un único botón "Empieza aquí" en la parte inferior que navega hacia `OnboardingView`.

**Estado:**
- `isPressed: Bool` — controla la animación de escala del botón.
- `navigateToOnboarding: Bool` — activa la navegación al onboarding.

---

## Onboarding

### `OnboardingView`
**Archivo:** `PRIMEFOODIST/INTRO/OnboardingView.swift`

Primera pantalla del onboarding. Muestra una imagen de hamburguesa, el titular *"Todos tus sabores favoritos"* y un subtítulo descriptivo. Incluye indicadores de página (3 círculos) y los botones **Siguiente** y **Volver**.

### `OnboardingView2`
**Archivo:** `PRIMEFOODIST/INTRO/OnboardingView2.swift`

Segunda pantalla del onboarding.

### `OnboardingView3`
**Archivo:** `PRIMEFOODIST/INTRO/OnboardingView3.swift`

Tercera y última pantalla del onboarding. El botón de acción final navega a `LoginView`.

---

## Autenticación

### `LoginView`
**Archivo:** `PRIMEFOODIST/LOGIN/LoginView.swift`

Pantalla de inicio de sesión. Contiene:
- Campo de correo electrónico con validación de caracteres en tiempo real.
- Campo de contraseña con toggle de visibilidad.
- Botón **Continuar** que llama a `Auth.auth().signIn` de Firebase.
- Enlace a `ForgotPasswordView`.
- Botones de acceso social (Google, Facebook, Apple) — actualmente abren las URLs de cada plataforma.
- Pestaña de acceso rápido a `RegisterView`.

**Validaciones:**
- Campo vacío (correo o contraseña).
- Formato de correo electrónico (`[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\.(es|com)`).
- Errores de Firebase: contraseña incorrecta, usuario no encontrado, cuenta desactivada, demasiados intentos, error de red.

**Componente interno:** `ErrorNotificationView` (banner de error animado en la parte superior).

### `ForgotPasswordView`
**Archivo:** `PRIMEFOODIST/LOGIN/ForgotPasswordView.swift`

Formulario para solicitar el restablecimiento de contraseña vía Firebase Auth (`sendPasswordReset`).

### `PasswordResetConfirmationView`
**Archivo:** `PRIMEFOODIST/LOGIN/PasswordResetConfirmationView.swift`

Pantalla de confirmación tras enviar el correo de restablecimiento.

---

## Registro

### `RegisterView`
**Archivo:** `PRIMEFOODIST/REGISTRO/RegisterView.swift`

Primer paso del registro. Recoge:
- **Edad** (solo números, campo numérico).
- **Sexo** (Hombre / Mujer / Prefiero no responder) mediante botones con estado de selección visual.

Valida que ambos campos estén completos antes de continuar a `RegisterFormView`.

**Enum interno:** `Gender { male, female, preferNotToSay }`

### `RegisterFormView`
**Archivo:** `PRIMEFOODIST/REGISTRO/RegisterFormView.swift`

Segundo paso. Recoge nombre, correo electrónico y contraseña. Crea el usuario en Firebase con `Auth.auth().createUser`. Incluye el componente `RFNotificationBanner` para feedback de errores y éxito.

### `UserTypeSelectionView`
**Archivo:** `PRIMEFOODIST/REGISTRO/UserTypeSelectionView.swift`

Permite al usuario seleccionar su tipo de perfil (p. ej. cliente habitual, profesional gastronómico…).

### `WelcomeCompletion` / `FinalCompletionView`
**Archivos:** `PRIMEFOODIST/REGISTRO/WelcomeCompletion.swift`, `FinalCompletionView.swift`

Pantallas de bienvenida y confirmación al final del flujo de registro.

### `RootView`
**Archivo:** `PRIMEFOODIST/REGISTRO/RootView.swift`

Vista raíz del flujo de registro que coordina la navegación entre los pasos.

---

## Home

### `HomeView`
**Archivo:** `PRIMEFOODIST/HOME/HomeView.swift`

Pantalla principal de la aplicación. Estructura:

1. **Cabecera** — "Bienvenido / Linares Gastro".
2. **Tendencias** — Scroll horizontal de `TrendingCard`.
3. **Categorías populares** — Filtros de texto (`CategoryButton`) + iconos emoji con navegación (`CategoryIcon`).
4. **Establecimientos** — Lista vertical de `EstablishmentCard`. El establecimiento *Cafetería Castulo* tiene `NavigationLink` a `DetalleEstablecimientoView`.
5. **Botón "Ver más"** — Navega a `MapaView`.
6. **Barra de navegación inferior** — `BottomNavigationBar` fija en la parte inferior.

### `NotificacionesView`
**Archivo:** `PRIMEFOODIST/HOME/NotificacionesView.swift`

Pantalla de notificaciones del usuario.

### `ReseniasView`
**Archivo:** `PRIMEFOODIST/HOME/ReseniasView.swift`

Lista de reseñas de un establecimiento.

### `AniadirReseniaView`
**Archivo:** `PRIMEFOODIST/HOME/AniadirReseniaView.swift`

Formulario para añadir una nueva reseña (valoración por estrellas + texto).

### `GalletasView`
**Archivo:** `PRIMEFOODIST/HOME/GalletasView.swift`

Vista de detalle del producto "Galletas" dentro de la Cafetería Castulo. Ejemplo de flujo de detalle de producto con opción de añadir al carrito.

---

## Detalle de establecimiento

### `DetalleEstablecimientoView`
**Archivo:** `PRIMEFOODIST/HOME/DetalleEstablecimientoView.swift`

Muestra la información completa de un establecimiento:
- Nombre y logo del establecimiento.
- Botón de seguir/dejar de seguir con animación.
- Imagen principal del local.
- Valoración media y número de reseñas.
- Enlace a `AniadirReseniaView`.
- **Menú** dividido por secciones (Bebidas, Panadería…) con `MenuItemCard`.
- Barra inferior `BottomNavigationBar`.

---

## Mapa

### `MapaView`
**Archivo:** `PRIMEFOODIST/HOME/MapaView.swift`

Vista de mapa interactivo centrado en Linares (`lat: 38.0693, lon: -3.6173`):
- Usa `Map` de MapKit con `showsUserLocation: true`.
- Scroll horizontal en la parte inferior con `RestauranteMapaCard` para cada restaurante.
- Navega a `DetalleEstablecimientoView` al pulsar una tarjeta.

**Modelos internos:**
- `RestauranteMapaInfo` — datos del restaurante (nombre, logo, distancia, rating, imágenes).

---

## Categorías

Cada vista de categoría muestra los establecimientos filtrados por tipo. Todas siguen la misma estructura de listado.

| Vista | Archivo | Emoji |
|---|---|---|
| `BurgersView` | `HOME/Categorias/Burgersview.swift` | 🍔 |
| `CafeteriasView` | `HOME/Categorias/Cafeteriasview.swift` | ☕ |
| `KebabsView` | `HOME/Categorias/Kebabsview.swift` | 🥙 |
| `BaresView` | `HOME/Categorias/Baresview.swift` | 🍺 |
| `HalalView` | `HOME/Categorias/Halalview.swift` | 🥗 |

---

## Carrito

### `CarritoView`
**Archivo:** `PRIMEFOODIST/HOME/carrito/CarritoView.swift`

Paso 1: muestra los productos seleccionados por el usuario con cantidades y precios.

### `Carrito2View`
**Archivo:** `PRIMEFOODIST/HOME/carrito/Carrito2View.swift`

Paso 2: resumen del pedido con dirección de entrega y método de pago.

### `Carrito3View`
**Archivo:** `PRIMEFOODIST/HOME/carrito/Carrito3View.swift`

Paso 3: confirmación del pedido y estado de envío.

---

## Reservas / Calendario

### `CalendarioView`
**Archivo:** `PRIMEFOODIST/HOME/calendario/CalendarioView.swift`

Vista de calendario para elegir fecha de reserva.

### `ReservaView`
**Archivo:** `PRIMEFOODIST/HOME/calendario/ReservaView.swift`

Paso 1 de reserva: selección de establecimiento y hora.

### `Reserva2View`
**Archivo:** `PRIMEFOODIST/HOME/calendario/Reserva2View.swift`

Paso 2: confirmación de datos de la reserva (número de personas, observaciones).

### `Reserva3View`
**Archivo:** `PRIMEFOODIST/HOME/calendario/Reserva3View.swift`

Paso 3: confirmación final de la reserva.

---

## Perfil

### `PerfilView`
**Archivo:** `PRIMEFOODIST/PERFIL/PerfilView.swift`

Perfil del usuario con cabecera en color teal y contenido blanco. Muestra:
- Avatar, nombre ("María García") y biografía.
- Estadísticas: reseñas, seguidores, siguiendo.
- Tres pestañas de contenido: **Reviews**, **Favoritos**, **Siguiendo**.
- Botón de edición → `EditarPerfilView`.
- Acceso a ajustes → `AjustesView`.

**Subvistas internas:**
- `EditarPerfilView` — formulario para editar nombre y biografía.
- `ReviewCard` — tarjeta de reseña (título, estrellas, texto).
- `FavoritoCard` — tarjeta de establecimiento favorito (título, descripción, imagen).
- `SiguiendoCard` — tarjeta de usuario seguido (nombre, bio, estadísticas).
- `StatItem` — par título/valor para las estadísticas del perfil.
- `FilterButton` — botón de pestaña con icono y texto.
- `RoundedCorner` — `Shape` personalizado para redondear esquinas específicas.

---

## Ajustes y cuenta

### `AjustesView`
**Archivo:** `PRIMEFOODIST/PERFIL/ajustesView.swift`

Menú de configuración general. Agrupa los siguientes accesos:

| Vista | Descripción |
|---|---|
| `CuentaView` | Datos de la cuenta (correo, contraseña) |
| `AccesibilidadView` | Opciones de accesibilidad (tamaño de texto, contraste) |
| `FAQView` | Preguntas frecuentes |
| `PromocionesView` | Ofertas y promociones activas |
| `HistorialPedidosView` | Historial de pedidos realizados |
| `HistorialReservasView` | Historial de reservas realizadas |

### `EliminarCuentaView`
**Archivo:** `PRIMEFOODIST/PERFIL/EliminarCuentaView.swift`

Flujo de confirmación para eliminar la cuenta del usuario.

### `HeaderSecundario`
**Archivo:** `PRIMEFOODIST/PERFIL/HeaderSecundario.swift`

Componente reutilizable de cabecera secundaria (barra de título con botón de volver) usado en las vistas de ajustes.

---

## Ubicación

### `LocationManager`
**Archivo:** `PRIMEFOODIST/PERMISOUBICACION/LocationManager.swift`

Clase `ObservableObject` que gestiona la geolocalización:
- Solicita el permiso `whenInUse`.
- Publica la coordenada actual en `@Published var userLocation`.
- Realiza geocodificación inversa con `CLGeocoder` y publica el nombre en `@Published var locationName`.

### `LocationSelectionView`
**Archivo:** `PRIMEFOODIST/PERMISOUBICACION/LocationSelectionView.swift`

Permite al usuario ver y seleccionar su ubicación actual.

### `AddLocationView`
**Archivo:** `PRIMEFOODIST/PERMISOUBICACION/AddLocationView.swift`

Formulario para introducir una dirección de entrega manualmente.

### `ChangeLocationView`
**Archivo:** `PRIMEFOODIST/PERMISOUBICACION/ChangeLocationView.swift`

Vista para cambiar la ubicación de entrega existente.

---

## Componentes reutilizables

### `ErrorNotificationView`
**Archivo:** `PRIMEFOODIST/LOGIN/LoginView.swift`

Banner de error animado que aparece en la parte superior de la pantalla. Se oculta automáticamente tras 3 segundos.

| Prop | Tipo | Descripción |
|---|---|---|
| `message` | `String` | Texto del error |
| `isVisible` | `Binding<Bool>` | Controla la visibilidad |

### `RFNotificationBanner`
**Archivo:** `PRIMEFOODIST/REGISTRO/RegisterFormView.swift`

Banner de notificación con soporte para cuatro tipos visuales: `.rfError` (rojo), `.rfSuccess` (teal), `.rfWarning` (naranja), `.rfInfo` (azul).

### `TrendingCard`
**Archivo:** `PRIMEFOODIST/HOME/HomeView.swift`

Tarjeta de tendencia con imagen (240×160), nombre del restaurante, tipo de comida y badge de rating.

| Prop | Tipo |
|---|---|
| `imageName` | `String` |
| `restaurantName` | `String` |
| `dishName` | `String` |
| `rating` | `Double` |

### `CategoryButton`
**Archivo:** `PRIMEFOODIST/HOME/HomeView.swift`

Píldora de filtro de texto (seleccionada en teal, no seleccionada en gris claro).

### `CategoryIcon`
**Archivo:** `PRIMEFOODIST/HOME/HomeView.swift`

Icono circular con emoji y texto inferior. Animación de escala al pulsar. Acepta un `action: () -> Void`.

### `EstablishmentCard`
**Archivo:** `PRIMEFOODIST/HOME/HomeView.swift`

Tarjeta vertical de establecimiento con imagen (altura 180), nombre, distancia, estado ("Abierto ahora") y badge de rating.

### `BottomNavigationBar`
**Archivo:** `PRIMEFOODIST/HOME/HomeView.swift`

Barra de navegación inferior fija con cuatro pestañas: Home, Calendario, Carrito y Perfil. Recibe `@Binding var selectedTab: Int` para sincronizar el icono activo (en color teal).

### `MenuItemCard`
**Archivo:** `PRIMEFOODIST/HOME/DetalleEstablecimientoView.swift`

Fila de item de menú con nombre, precio e icono de bolsa de compra. Fondo teal semitransparente.

### `RestauranteMapaCard`
**Archivo:** `PRIMEFOODIST/HOME/MapaView.swift`

Tarjeta flotante sobre el mapa (ancho 320) con logo del restaurante, nombre, distancia, estrellas de rating y scroll horizontal de imágenes.

### `ReviewCard`
**Archivo:** `PRIMEFOODIST/PERFIL/PerfilView.swift`

Tarjeta de reseña (180pt de alto) con título, estrellas y texto de la reseña.

### `FavoritoCard`
**Archivo:** `PRIMEFOODIST/PERFIL/PerfilView.swift`

Tarjeta de favorito (220pt de alto) con título, descripción e imagen del establecimiento.

### `SiguiendoCard`
**Archivo:** `PRIMEFOODIST/PERFIL/PerfilView.swift`

Tarjeta de usuario seguido (220pt de alto) con avatar, nombre, estado de seguimiento mutuo, estadísticas y biografía.

### `StatItem`
**Archivo:** `PRIMEFOODIST/PERFIL/PerfilView.swift`

Componente de estadística: par título (pequeño, gris) + valor (grande, negro).

### `FilterButton`
**Archivo:** `PRIMEFOODIST/PERFIL/PerfilView.swift`

Botón de pestaña con icono SF Symbol y texto. Fondo teal cuando está seleccionado.

### `RoundedCorner`
**Archivo:** `PRIMEFOODIST/PERFIL/PerfilView.swift`

`Shape` personalizado que aplica radio de esquina solo a las esquinas indicadas en `UIRectCorner`. Usado para la separación visual entre el header teal y el contenido blanco del perfil.
