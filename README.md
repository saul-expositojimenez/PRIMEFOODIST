# PRIMEFOODIST 

**PRIMEFOODIST** es una aplicación iOS de gastronomía local centrada en la ciudad de **Linares (Jaén, España)**. Permite a los usuarios descubrir restaurantes, cafeterías, bares y otros establecimientos de hostelería cercanos, ver sus menús, hacer reservas, gestionar pedidos y dejar reseñas.

---

##  Capturas de pantalla

La carpeta [`IMG/`](IMG/) contiene los recursos gráficos utilizados en la aplicación, incluyendo logos de establecimientos y fotografías de platos.

---

##  Funcionalidades principales

| Módulo | Descripción |
|---|---|
| **Intro** | Pantalla de bienvenida con onboarding de 3 pasos |
| **Autenticación** | Inicio de sesión y registro con Firebase Auth |
| **Home** | Listado de establecimientos con tendencias y categorías |
| **Categorías** | Filtrado por tipo: Burgers, Cafeterías, Kebabs, Bares, Halal |
| **Detalle de establecimiento** | Menú, reseñas, seguimiento y galería |
| **Mapa** | Vista de mapa con restaurantes cercanos en Linares |
| **Carrito** | Flujo completo de pedido en 3 pasos |
| **Reservas** | Calendario para gestionar reservas en establecimientos |
| **Perfil** | Reseñas, favoritos, seguidores y configuración de cuenta |
| **Ubicación** | Gestión de permisos y localización con CoreLocation |

---

##  Tecnologías utilizadas

- **SwiftUI** — Framework de UI declarativo de Apple
- **Firebase Auth** — Autenticación de usuarios (correo/contraseña)
- **FirebaseCore** — Configuración del SDK de Firebase
- **MapKit / CoreLocation** — Mapa interactivo y geolocalización
- **Combine** — Programación reactiva para actualizaciones de ubicación
- **Xcode** — Entorno de desarrollo (versión mínima compatible: Xcode 15+)
- **iOS 17+** — Versión mínima de sistema operativo

---

##  Estructura del proyecto

```
PRIMEFOODIST/
├── PRIMEFOODISTApp.swift          # Punto de entrada; inicializa Firebase
├── ContentView.swift              # Vista raíz con pantalla de inicio
│
├── INTRO/                         # Flujo de onboarding (3 pantallas)
│   ├── OnboardingView.swift
│   ├── OnboardingView2.swift
│   └── OnboardingView3.swift
│
├── LOGIN/                         # Autenticación
│   ├── LoginView.swift
│   ├── ForgotPasswordView.swift
│   └── PasswordResetConfirmationView.swift
│
├── REGISTRO/                      # Flujo de registro de nuevos usuarios
│   ├── RegisterView.swift         # Paso 1: edad y sexo
│   ├── RegisterFormView.swift     # Paso 2: datos de cuenta
│   ├── UserTypeSelectionView.swift
│   ├── WelcomeCompletion.swift
│   ├── FinalCompletionView.swift
│   └── RootView.swift
│
├── HOME/                          # Pantalla principal
│   ├── HomeView.swift             # Listado, tendencias y categorías
│   ├── DetalleEstablecimientoView.swift
│   ├── MapaView.swift
│   ├── NotificacionesView.swift
│   ├── ReseniasView.swift
│   ├── AniadirReseniaView.swift
│   ├── GalletasView.swift
│   │
│   ├── Categorias/                # Vistas por categoría de establecimiento
│   │   ├── BurgersView.swift
│   │   ├── CafeteriasView.swift
│   │   ├── KebabsView.swift
│   │   ├── BaresView.swift
│   │   └── HalalView.swift
│   │
│   ├── carrito/                   # Flujo de carrito de compra
│   │   ├── CarritoView.swift
│   │   ├── Carrito2View.swift
│   │   └── Carrito3View.swift
│   │
│   └── calendario/                # Gestión de reservas
│       ├── CalendarioView.swift
│       ├── ReservaView.swift
│       ├── Reserva2View.swift
│       └── Reserva3View.swift
│
├── PERFIL/                        # Perfil de usuario
│   ├── PerfilView.swift
│   ├── CuentaView.swift
│   ├── ajustesView.swift
│   ├── AccesibilidadView.swift
│   ├── FAQView.swift
│   ├── PromocionesView.swift
│   ├── HistorialPedidosView.swift
│   ├── HistorialReservasView.swift
│   ├── EliminarCuentaView.swift
│   └── HeaderSecundario.swift
│
├── PERMISOUBICACION/              # Gestión de ubicación
│   ├── LocationManager.swift      # CLLocationManager observable
│   ├── LocationSelectionView.swift
│   ├── AddLocationView.swift
│   └── ChangeLocationView.swift
│
├── Tipografia/                    # Fuentes personalizadas
│   └── Inter, Manrope, Poppins, Raleway
│
└── Assets.xcassets                # Imágenes y colores del asset catalog
```

---

##  Configuración e instalación

Ver [`docs/CONFIGURACION.md`](docs/CONFIGURACION.md) para instrucciones detalladas de configuración del entorno y Firebase.

---

##  Arquitectura

Ver [`docs/ARQUITECTURA.md`](docs/ARQUITECTURA.md) para una descripción completa de la arquitectura y el flujo de navegación.

---

##  Vistas y componentes

Ver [`docs/VISTAS.md`](docs/VISTAS.md) para la documentación detallada de cada vista y componente reutilizable.

---

##  Tests

El proyecto incluye una suite de tests básica en `PRIMEFOODISTTests/PRIMEFOODISTTests.swift`. Para ejecutarlos:

1. Abre el proyecto en Xcode.
2. Selecciona el esquema `PRIMEFOODISTTests`.
3. Pulsa `Cmd + U`.

---

##  Autores

Desarrollado como proyecto de presentación por el equipo DAM1 (curso 2025-2026).
