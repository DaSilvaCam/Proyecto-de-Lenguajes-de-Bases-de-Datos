# Práctica #1 — Repaso integrador con TiendaApp

## Información general

| Dato | Valor |
|------|-------|
| Curso | SC-403 Desarrollo de Aplicaciones Web y Patrones |
| Universidad | Fidélitas |
| Modalidad | Individual, asincrónica |
| Valor | **5%** de la nota final |
| Tiempo estimado | 2 a 3 horas |
| Fecha de asignación | Martes 16 de junio 2026 (grupo martes) / Viernes 19 de junio 2026 (grupo viernes) |
| Fecha de entrega | **8 días** después de la asignación |
| Fecha límite — Grupo Martes | **Martes 23 de junio 2026 a las 23:59** |
| Fecha límite — Grupo Viernes | **Viernes 26 de junio 2026 a las 23:59** |

> Después de la fecha límite, la entrega aplica como tardía según el reglamento de la universidad.

---

## Propósito

Esta primera práctica busca que **verifiqués que tu entorno de desarrollo está completo y funcionando** y que **podés integrar lo aprendido en las Clases 1 a 4** (Git, Spring Boot, Thymeleaf, MySQL/JPA) en una sola aplicación.

No es una práctica difícil. La idea es que ganés confianza con el flujo completo antes de avanzar con CRUD y seguridad en las próximas semanas. Si tenés todo instalado y comprendiste el patrón MVC, podés terminar sin problemas.

---

## Objetivos de aprendizaje

Al terminar esta práctica vas a haber demostrado que sos capaz de:

1. Configurar y correr un proyecto Spring Boot en tu máquina.
2. Conectar la aplicación a una base de datos MySQL local con variables de entorno.
3. Modificar una vista Thymeleaf usando componentes de Bootstrap.
4. Aplicar el workflow Git básico (`pull → add → commit → push`) sobre tu repositorio personal del curso.

---

## Material entregado

Dentro de este paquete vas a encontrar:

| Archivo | Descripción |
|---------|-------------|
| `practica_1.md` | Este documento con instrucciones y rúbrica. |
| `tiendaapp.zip` | Proyecto Spring Boot completo construido por el profesor. |

El proyecto `tiendaapp` ya tiene:

- Entity `Producto` con JPA.
- Repository con query methods.
- Service con inyección de dependencias.
- 2 Controllers (Home + Producto).
- Vistas Thymeleaf con grid responsive de Bootstrap.
- Script `seed-data.sql` con 8 productos de ejemplo.

---

## Antes de empezar

Verificá que tenés todo instalado y funcionando:

```bash
java --version    # OpenJDK 21
git --version     # 2.x
mvn --version     # opcional, el wrapper viene en el proyecto
mysql --version   # 8.x (o tener MySQL Workbench abierto)
```

Abrí:

- VS Code.
- MySQL Workbench con tu conexión local.
- Tu repositorio personal del curso clonado en disco.
- GitHub con sesión iniciada.

---

## Lo que tenés que hacer

### Paso 1 — Colocar el proyecto en tu repositorio

Posicionate en tu repositorio personal del curso y traete los últimos cambios:

```bash
cd ruta\a\tu\repositorio-del-curso
git pull
```

Creá una carpeta para esta práctica dentro de tu repo, siguiendo la convención que ya venís usando (por ejemplo `Practica_1` o `Clase_6_Practica1`):

```bash
mkdir Practica_1
```

Descomprimí `tiendaapp.zip` dentro de esa carpeta. La estructura tiene que quedar:

```
tu-repositorio-del-curso/
├── Clase_1/
├── Clase_2/
├── ...
└── Practica_1/
    └── tiendaapp/
        ├── pom.xml
        ├── src/
        ├── seed-data.sql
        └── ...
```

### Paso 2 — Configurar MySQL y la contraseña

Creá la base de datos en MySQL Workbench:

```sql
CREATE DATABASE tiendadb
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
```

Definí tu contraseña con `setx` (queda persistente, solo se ejecuta una vez):

```powershell
setx DB_PASSWORD "tu_password_de_mysql"
```

Cerrá la terminal y abrí una nueva para que tome efecto. Verificá con:

```powershell
echo $env:DB_PASSWORD
```

Como respaldo, si llegás a necesitar guardar la contraseña en un archivo, creá `src/main/resources/application-local.properties` con:

```properties
spring.datasource.password=tu_password_de_mysql
```

El proyecto ya tiene `application-local.properties` en su `.gitignore`, así que no se va a subir al repositorio.

### Paso 3 — Correr la aplicación y cargar datos

```bash
cd Practica_1\tiendaapp
.\mvnw.cmd spring-boot:run     # Windows
./mvnw spring-boot:run         # Linux o Mac
```

Esperá a ver `Started TiendaappApplication in X seconds`.

En MySQL Workbench, abrí el archivo `seed-data.sql` que viene en la raíz del proyecto y ejecutalo. Debe insertar 8 productos.

Verificá:

- http://localhost:8080 muestra la home.
- http://localhost:8080/productos muestra los 8 productos en un grid.
- http://localhost:8080/productos/1 muestra el detalle de un producto.

### Paso 4 — Aplicar un cambio en la vista

Editá `src/main/resources/templates/productos.html`. Dentro del bloque que dibuja cada card de producto, agregá los siguientes badges:

```html
<span class="badge bg-info mb-2" th:text="${producto.categoria}">categoria</span>

<span th:if="${producto.bajoStock}"
      class="badge bg-warning text-dark">
  Pocas unidades
</span>

<span th:if="${producto.agotado}"
      class="badge bg-danger">
  Agotado
</span>
```

Recargá la página. Cada card debe mostrar ahora la categoría del producto y, según los datos, una alerta de stock bajo o de agotado.

### Paso 5 — Subir el resultado a tu repositorio

Desde la raíz de tu repositorio personal:

```bash
git pull
git status
```

Verificá que `application-local.properties` **NO** aparece en la lista (si aparece, revisá el `.gitignore` antes de continuar).

```bash
git add .
git commit -m "feat(practica1): TiendaApp con badges de categoria y stock"
git push
```

### Paso 6 — Capturar evidencia

Tomá una captura de pantalla del navegador con `localhost:8080/productos` mostrando al menos un producto con badge de categoría visible y al menos uno con la alerta amarilla o roja.

Guardá la captura como `Practica_1/evidencia.png` dentro de tu repositorio. Hacé un nuevo commit y push:

```bash
git add Practica_1/evidencia.png
git commit -m "docs(practica1): captura de evidencia"
git push
```

---

## Cómo entregar

Subí al campus virtual (Moodle), en el espacio designado para esta práctica:

1. **URL de tu repositorio de GitHub** completa (`https://github.com/tu-usuario/tu-repo`).
2. **Hash del último commit** (los primeros 7 caracteres, los podés ver con `git log --oneline -1`).

Ejemplo:

```
URL: https://github.com/tu-usuario/SC-403-DesarrolloWeb
Commit: a3b7f29
```

No subas el código ni la captura al campus virtual. Toda la revisión la hacemos directamente en GitHub.

---

## Rúbrica de evaluación (5%)

La nota se asigna sobre 100 y luego se multiplica por 0.05 para obtener el porcentaje final.

| Criterio | Peso | Excelente (100) | Aceptable (70) | Insuficiente (40) | No entregado (0) |
|----------|:----:|:----------------|:---------------|:------------------|:-----------------|
| **1. Estructura del repositorio** | 15 | Carpeta `Practica_1/tiendaapp/` correctamente ubicada dentro del repo. | La carpeta existe pero el nombre o la ubicación no siguen la convención. | El proyecto está en la raíz del repo o desorganizado. | No hay carpeta de la práctica. |
| **2. Proyecto completo subido** | 20 | Todo `tiendaapp/` está en el repo: pom, src, seed-data.sql, README. | Falta algún archivo no crítico. | Faltan archivos clave (src o pom). | El proyecto no está subido. |
| **3. Cambios aplicados en la vista** | 25 | `productos.html` tiene los 3 badges (categoría, pocas unidades, agotado) bien aplicados. | Tiene solo el badge de categoría. | Hay cambios pero rompen la vista o no se ven los badges. | No hay cambios. |
| **4. Workflow Git correcto** | 15 | Al menos 2 commits con mensajes claros (convención `feat:` / `docs:` / `chore:`). | Hay commits pero mensajes vagos (`update`, `wip`). | Un solo commit con todo amontonado. | No hay commits desde el push inicial. |
| **5. Captura de evidencia** | 10 | `evidencia.png` clara, muestra los badges visibles. | Captura existe pero los badges no se ven bien. | Captura no relacionada con la práctica. | No hay captura. |
| **6. Seguridad básica** | 15 | `application-local.properties` y archivos similares NO están en el repo. | Hay un archivo de configuración local subido por error pero sin contraseñas reales. | Hay alguna contraseña visible en algún archivo. | Hay credenciales reales subidas al repo público. |

**Penalizaciones automáticas:**

- Si subiste tu contraseña real de MySQL al repositorio, la nota máxima de la práctica es 60. Esto se cuenta como un incidente de seguridad — registralo en un commit nuevo eliminando la credencial y cambiando tu contraseña local.
- Si entregás después de la fecha límite, se aplica el reglamento de evaluaciones tardías de la universidad.

---

## Conceptos repasados

Esta práctica integra los siguientes conceptos vistos en clase:

| Clase | Conceptos |
|-------|-----------|
| Clase 1 | Git: `pull`, `add`, `commit`, `push`, mensajes con conventional commits. |
| Clase 2 | Spring Boot arrancando con Maven Wrapper, `application.properties`, variables de entorno, DevTools. |
| Clase 3 | Thymeleaf (`th:text`, `th:if`, `th:each`), componentes Bootstrap (badges, grid, cards). |
| Clase 4 | JPA con `@Entity`, JpaRepository con query methods, Service con inyección de dependencias, Controller siguiendo MVC. |

---

## Problemas comunes

| Síntoma | Solución |
|---------|----------|
| `Access denied for user 'root'` al arrancar | Cerrá y abrí una terminal nueva para que tome la `DB_PASSWORD`. Verificá con `echo $env:DB_PASSWORD`. |
| `Unknown database 'tiendadb'` | Creá la base de datos con el SQL del Paso 2. |
| Whitelabel Error Page en `/productos` | Revisá que el método `listar()` del Controller retorne el nombre correcto de la vista (`"productos"`, sin `.html`). |
| Los badges no aparecen | Verificá que guardaste `productos.html` y refrescá el navegador. Si no está DevTools, reiniciá la aplicación. |
| `git push` falla con "rejected" | Hacé `git pull` antes de pushear. Si igual falla, revisá la rama con `git branch`. |
| Subí `application-local.properties` por error | Borralo del repo, agregalo al `.gitignore` si no está, commiteá y aprovechá para cambiar tu contraseña de MySQL local. |

---

## Recursos de consulta

| Tema | Enlace |
|------|--------|
| Spring Data JPA | https://docs.spring.io/spring-data/jpa/reference/ |
| Thymeleaf | https://www.thymeleaf.org/documentation.html |
| Bootstrap 5 | https://getbootstrap.com/docs/5.3 |
| Git book (en español) | https://git-scm.com/book/es/v2 |
| Conventional Commits | https://www.conventionalcommits.org/es |

---

## Preguntas

Cualquier duda durante el desarrollo de la práctica podés consultarla en:

- El canal `Consultas` del equipo de Teams.
- Las horas de consulta semanales publicadas en el espacio de la materia.

No se aceptan consultas vía correo personal una vez iniciada la práctica.
