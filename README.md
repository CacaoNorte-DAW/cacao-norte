# Cacao Norte

Aplicación web de gestión para **Cacao Norte S.L.**, una empresa ficticia de chocolate de especialidad «bean to bar» con sede en Toledo.

Proyecto Intermodular · 2º DAW · IES Azarquiel · Curso 2026/2027

## Equipo

| Integrante | Rol principal |
| --- | --- |
| Adrián Lozano Díaz | Backend y entorno de trabajo |
| Carlos Gómez de la Torre Bretón | Responsable de backend |
| Adrián Peña Gómez | Frontend y diseño |

Los tres participamos en la base de datos, las pruebas y la coordinación del proyecto.

## Qué hace la aplicación

**Parte pública (clientes)**
- Catálogo de chocolates con ficha técnica (origen, variedad de cacao, porcentaje, obrador, notas de cata y alérgenos) y filtros.
- Tienda online con carrito y pago simulado.
- Registro de usuarios.
- Club de suscripción mensual.
- Reserva de plazas para catas y talleres.

**Parte privada (panel de administración)**
- Stock por lotes con avisos de consumo preferente.
- Pedidos online y de hostelería.
- Clientes, suscriptores, proveedores y empleados.
- Facturas y calendario de catas y talleres.
- Acceso por roles: dirección, administración, sala, tienda y almacén.

## Tecnologías

| Capa | Tecnología |
| --- | --- |
| Frontend | HTML5, CSS3, JavaScript, Bootstrap 5 |
| Backend | Java 21, Spring Boot, Spring Data JPA, Spring Security, Maven |
| Base de datos | Oracle Database (local, con SQL Developer) y Oracle Autonomous Database en Oracle Cloud (producción) |
| Pagos | Stripe en modo pruebas |
| Servidor | Linux + Nginx |

## Estructura del repositorio

```
cacao-norte/
├── backend/    API REST con Spring Boot
├── frontend/   Parte pública y panel de administración
├── db/         Scripts SQL de la base de datos
└── docs/       Documentación del proyecto, Postman e informe de pruebas
```

## Cómo trabajamos con Git

**Ramas**
- `main`: versión estable, la que se despliega.
- `develop`: rama de integración. Es la rama por defecto.
- `feature/nombre-tarea`: una rama por cada tarea, creada desde `develop`.

**Flujo de trabajo**
1. Actualiza `develop`: `git checkout develop` y `git pull`.
2. Crea tu rama: `git checkout -b feature/nombre-tarea`.
3. Haz commits pequeños y con mensajes claros.
4. Sube la rama: `git push -u origin feature/nombre-tarea`.
5. Abre un pull request hacia `develop`.
6. Otro compañero lo revisa y lo aprueba antes de fusionarlo.

`main` y `develop` están protegidas: no se puede subir directamente a ellas, todo entra mediante pull request con al menos una aprobación.

**Mensajes de commit**
Empieza por el tipo de cambio:
- `feat:` nueva funcionalidad (`feat: listado de chocolates en el catálogo`)
- `fix:` corrección de un error
- `docs:` documentación
- `refactor:` mejora del código sin cambiar su funcionamiento
- `test:` pruebas

## Cómo arrancar el proyecto en local

> Se completará cuando esté creado el proyecto base.

**Requisitos:** Java 21, Maven, Git, Oracle Database en local y Oracle SQL Developer.

Las contraseñas y claves (base de datos, Stripe…) van en archivos locales que **nunca se suben** al repositorio.

Para conectar con la base de datos de Oracle Cloud hace falta el *wallet*. Cada uno lo guarda **fuera de la carpeta del repo** y nunca se sube.
