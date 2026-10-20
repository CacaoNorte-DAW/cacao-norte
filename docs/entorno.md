# Entorno de trabajo

Guía para poner en marcha el proyecto y decisiones técnicas tomadas al montar el entorno.

> En este documento no hay contraseñas ni claves. Cada integrante las tiene guardadas por su cuenta.

## 1. Repositorio en GitHub

- **Organización:** [CacaoNorte-DAW](https://github.com/CacaoNorte-DAW). Los tres integrantes somos propietarios (*Owner*) y cada uno entra con su propia cuenta de GitHub.
- **Repositorio:** `cacao-norte`, público.
- **Tablero de tareas:** GitHub Projects de la organización.

### Ramas

| Rama | Uso |
| --- | --- |
| `main` | Versión estable, la que se despliega. |
| `develop` | Rama de integración. Es la rama por defecto. |
| `feature/nombre-tarea` | Una rama por tarea, creada desde `develop`. |

### Reglas de protección

`main` y `develop` tienen un *ruleset* activo (`proteccion-ramas`):
- No se puede subir directamente: todo entra mediante **pull request**.
- Cada pull request necesita **1 aprobación** de otro integrante.
- Si se suben cambios nuevos a un pull request ya aprobado, la aprobación se pierde y hay que revisarlo de nuevo.
- No se puede borrar la rama ni hacer *force push*.

### Clonar el repositorio

```bash
git clone https://github.com/CacaoNorte-DAW/cacao-norte.git
cd cacao-norte
git checkout develop
```

Para trabajar en una tarea:

```bash
git checkout develop
git pull
git checkout -b feature/nombre-tarea
# ... cambios y commits ...
git push -u origin feature/nombre-tarea
```

Después se abre el pull request hacia `develop` desde GitHub.

## 2. Base de datos

### Decisión: Oracle en lugar de MySQL

Al principio planteamos MySQL, pero usamos **Oracle Database** porque:
- Es el sistema gestor que ya usamos en otros módulos del ciclo, con SQL Developer.
- Spring Boot funciona igual con Oracle: solo cambian el driver (`ojdbc`) y la configuración de conexión.
- Oracle Cloud ofrece una base de datos gratuita (*Always Free*) para producción, sin tener que instalar Oracle en el servidor.

### Desarrollo (local)

Cada integrante trabaja con su Oracle local y **SQL Developer**. Los scripts de creación de tablas y los datos de prueba se guardan en la carpeta `db/`, para que los tres tengamos la misma base de datos.

### Producción (Oracle Cloud)

| Dato | Valor |
| --- | --- |
| Servicio | Oracle Autonomous Database, tipo *Transaction Processing* |
| Plan | Always Free |
| Región | Spain Central (Madrid) |
| Nombre | `cacaonorte` |
| Versión | 26ai |
| Conexión | Con *wallet* (mTLS obligatorio), servicio `cacaonorte_tp` |

**Usuarios:**
- `ADMIN`: solo para administrar (crear usuarios, dar permisos).
- `CACAONORTE`: usuario de la aplicación. Las tablas del proyecto se crean en este esquema y la aplicación se conecta con él.

Permisos del usuario `CACAONORTE`:

```sql
GRANT CREATE SESSION, CREATE TABLE, CREATE SEQUENCE, CREATE VIEW,
      CREATE TRIGGER, CREATE PROCEDURE TO CACAONORTE;
ALTER USER CACAONORTE QUOTA UNLIMITED ON DATA;
```

**A tener en cuenta:**
- Si la base de datos pasa 7 días sin actividad, Oracle la para automáticamente. Los datos se conservan y se vuelve a arrancar desde la consola de Oracle Cloud.
- Si sigue parada 3 meses, Oracle la elimina.
- Las copias de seguridad automáticas se conservan 60 días.

### El wallet

El wallet es el archivo `.zip` que permite conectarse a la base de datos de Oracle Cloud. Se descarga desde la consola en **Database connection → Download wallet → Instance wallet**.

- **Nunca se guarda dentro de la carpeta del repositorio.** El `.gitignore` lo ignora por seguridad, pero aun así se guarda fuera.
- Para SQL Developer se usa el `.zip` sin descomprimir.
- Para la aplicación de Spring Boot hará falta una copia descomprimida (se documentará al configurar la conexión).

### Conectarse desde SQL Developer

Nueva conexión (botón **+** verde):

| Campo | Valor |
| --- | --- |
| Name | `cacaonorte-app` |
| Username | `CACAONORTE` |
| Password | la del usuario `CACAONORTE` |
| Connection Type | Cloud Wallet |
| Configuration File | el `.zip` del wallet |
| Service | `cacaonorte_tp` |

Comprobación: `SELECT USER FROM DUAL;` debe devolver `CACAONORTE`.

## 3. Pendiente de decidir

- **Docker:** está previsto en la planificación, pero aún no hemos decidido si lo usamos.
- **Despliegue:** servidor propio en Hetzner con Linux y Nginx (aprobado por el profesor).
