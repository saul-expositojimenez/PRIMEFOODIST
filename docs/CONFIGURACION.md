# Configuración del proyecto PRIMEFOODIST

Esta guía explica cómo configurar el entorno de desarrollo para ejecutar PRIMEFOODIST en local.

---

## Requisitos previos

| Herramienta | Versión mínima |
|---|---|
| macOS | Ventura 13.0 o superior |
| Xcode | 15.0 o superior |
| iOS Simulator / Dispositivo | iOS 17.0 o superior |
| Cuenta de Firebase | Gratuita (plan Spark) |

---

## 1. Clonar el repositorio

```bash
git clone https://github.com/M4rt0-dev/Presentacion.git
cd Presentacion
```

---

## 2. Configurar Firebase

El proyecto usa **Firebase Authentication** para la gestión de usuarios. Es necesario vincular un proyecto de Firebase propio:

### 2.1 Crear proyecto en Firebase Console

1. Accede a [https://console.firebase.google.com](https://console.firebase.google.com).
2. Crea un nuevo proyecto (o usa uno existente).
3. Añade una aplicación iOS con el Bundle ID del proyecto (`com.tuempresa.PRIMEFOODIST` o el que tengas configurado en Xcode).
4. Descarga el archivo `GoogleService-Info.plist` generado.

### 2.2 Añadir el archivo de configuración

Copia el `GoogleService-Info.plist` descargado dentro de la carpeta:

```
PRIMEFOODIST/LOGIN/GoogleService-Info.plist
```

>  El archivo `GoogleService-Info.plist` ya existe en la ruta indicada (incluido en el proyecto de Xcode). Sustitúyelo por el tuyo sin cambiar el nombre ni la ubicación.

### 2.3 Activar método de autenticación

En Firebase Console, ve a **Authentication → Sign-in method** y activa:

-  Correo electrónico / Contraseña

---

## 3. Abrir el proyecto en Xcode

```bash
open PRIMEFOODIST.xcodeproj
```

> Si el proyecto usara Swift Package Manager o CocoaPods para las dependencias de Firebase, sigue los pasos adicionales del apartado siguiente. En caso de que las dependencias ya estén resueltas, este paso no es necesario.

---

## 4. Dependencias de Firebase (Swift Package Manager)

El proyecto integra Firebase a través de **Swift Package Manager**. Si las dependencias no se resuelven automáticamente al abrir el proyecto:

1. En Xcode, ve a **File → Packages → Resolve Package Versions**.
2. Xcode descargará e instalará:
   - `FirebaseCore`
   - `FirebaseAuth`

Si necesitas añadirlas manualmente:

1. Ve a **File → Add Package Dependencies…**
2. Introduce la URL: `https://github.com/firebase/firebase-ios-sdk`
3. Selecciona la versión estable más reciente.
4. Añade los productos `FirebaseCore` y `FirebaseAuth` al target `PRIMEFOODIST`.

---

## 5. Fuentes personalizadas

El proyecto usa las siguientes fuentes de Google Fonts, incluidas en la carpeta `Tipografia/`:

- **Poppins** (Regular, Medium, SemiBold, Bold)
- **Inter**
- **Manrope**
- **Raleway**

Las fuentes están declaradas en `Info.plist` bajo la clave `UIAppFonts`. Si añades nuevas variantes:

1. Arrastra el archivo `.ttf` o `.otf` a la carpeta `Tipografia/` dentro de Xcode (marcando "Add to target: PRIMEFOODIST").
2. Añade el nombre exacto del archivo (p. ej. `Poppins-Light.ttf`) a la clave `UIAppFonts` en `Info.plist`.

---

## 6. Permisos requeridos (Info.plist)

El proyecto requiere los siguientes permisos declarados en `Info.plist`:

| Clave | Descripción |
|---|---|
| `NSLocationWhenInUseUsageDescription` | Necesario para mostrar restaurantes cercanos en el mapa |

---

## 7. Ejecutar la aplicación

1. Selecciona un simulador (iPhone 15 Pro recomendado) o conecta un dispositivo físico.
2. Pulsa **Run** (`Cmd + R`) en Xcode.
3. La primera vez que se ejecuta, Firebase se inicializa automáticamente en `AppDelegate.application(_:didFinishLaunchingWithOptions:)`.

---

## 8. Variables de entorno y secretos

>  **No subas nunca el archivo `GoogleService-Info.plist` a un repositorio público.** Contiene claves API de Firebase que podrían ser utilizadas de forma maliciosa.

Para proyectos de equipo, se recomienda:

- Añadir `GoogleService-Info.plist` al `.gitignore`.
- Compartir el archivo de forma segura (p. ej. mediante un gestor de secretos o enviándolo por canal cifrado).
