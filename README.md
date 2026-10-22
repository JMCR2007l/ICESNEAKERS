# ICE sneakers — tienda completa (Spring Boot + frontend)

Tu proyecto HTML convertido en una aplicación Java con backend, base de datos y frontend, conservando tu diseño (`estilos.css`, logo, íconos e imágenes).

## Cómo abrirlo en IntelliJ IDEA

1. **File → Open…** y elige la carpeta `icesneakers-java` (la que contiene `pom.xml`). Acepta *Trust Project*.
2. IntelliJ detecta Maven y descarga las dependencias solo (necesita internet la primera vez).
3. Configura el JDK: **File → Project Structure → Project → SDK = JDK 17 o superior** (funciona con 17, 21 o 25).
4. Abre `src/main/java/com/icesneakers/IceSneakersApplication.java` y pulsa el botón ▶ junto a `main`.
5. Entra a **http://localhost:8080**

Desde terminal también sirve: `mvn spring-boot:run`

Para correr las pruebas: `mvn test`

## Cuentas de prueba

| Rol | Correo | Contraseña |
|---|---|---|
| Administrador | admin@icesneakers.com | Admin1234 |
| Cliente | cliente@demo.com | Cliente123 |

Cupones: `ICE10` (10 %) y `BIENVENIDO` (5 %). Envío gratis desde $10,000 (si no, $199).

## Qué incluye

**Frontend** (`src/main/resources/static`): inicio, catálogo con filtros/orden/paginación, ficha de producto con tallas, carrito, checkout, login/registro, mis pedidos y panel de administración. Responsive (menú móvil).

**Backend** (`src/main/java/com/icesneakers`):
- API REST en `/api/...`: productos, carrito, auth, pedidos y admin.
- Base de datos H2 en archivo (`./data/`), creada y poblada sola con tus 8 tenis + el Dunk ICE. Consola en `/h2-console` (JDBC URL: `jdbc:h2:file:./data/icesneakers`, usuario `sa`, sin contraseña).
- Carrito en sesión (invitados y usuarios), cupones, cálculo de envío.
- Checkout transaccional: valida stock, lo descuenta con bloqueo y guarda el pedido con precios congelados. Cancelar un pedido devuelve el stock.
- Roles CLIENTE / ADMIN. Contraseñas con BCrypt, cookie de sesión HttpOnly + SameSite, bloqueo temporal tras 5 intentos fallidos, mensajes de error uniformes en JSON, escape de HTML en el frontend.
- Panel admin: estadísticas, alta/edición/baja de productos, cambio de estado de pedidos.

## Estructura

```
icesneakers-java/
├── pom.xml
├── original-html/            ← tu HTML original, por referencia
└── src/
    ├── main/java/com/icesneakers/
    │   ├── model/   (Product, AppUser, CustomerOrder, OrderItem, Enums)
    │   ├── repo/    (repositorios Spring Data)
    │   ├── service/ (ProductService, CartService, OrderService, AuthService)
    │   ├── web/     (controladores REST + manejo global de errores)
    │   ├── dto/     (objetos de entrada/salida)
    │   └── config/  (DataSeeder: datos iniciales)
    ├── main/resources/
    │   ├── application.properties
    │   └── static/  (HTML, CSS, JS, imágenes)
    └── test/java/   (pruebas de la API)
```

## Cosas a revisar

- **Dunk ICE**: tu HTML no tenía precio para el Dunk ICE de la promo; puse $4,999 (antes $6,499) y 100 piezas. Cámbialo en el panel admin.
- **Género de cada tenis** (Hombre/Mujer/Unisex) y **stock** son valores de ejemplo en `DataSeeder.java`. «Niños» no tiene productos propios aún; los unisex aparecen ahí.
- El **pago es simulado** (no se cobra ni se piden datos de tarjeta). Para cobrar de verdad habría que integrar Stripe o Mercado Pago.
- Si cambias el modelo de datos y ya existe `./data/`, borra esa carpeta para recrear la base.
- Para agregar fotos nuevas, copia el archivo a `src/main/resources/static/imagenes/` y usa esa ruta en el panel admin.
