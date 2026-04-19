# EduGame - Sistema de Juegos Didácticos

Aplicación web educativa desarrollada en Java con Spring Boot, orientada a estudiantes de primaria para reforzar su aprendizaje mediante juegos interactivos tipo quiz.

## Tabla de Contenidos

- [Requisitos Previos](#requisitos-previos)
- [Instalación del Entorno](#instalación-del-entorno)
- [Configuración del Proyecto](#configuración-del-proyecto)
- [Estructura de Ramas y Flujo de Trabajo](#estructura-de-ramas-y-flujo-de-trabajo)
- [Ejecución de la Aplicación](#ejecución-de-la-aplicación)
- [Solución de Problemas Comunes](#solución-de-problemas-comunes)
- [Tecnologías Utilizadas](#tecnologías-utilizadas)

## Requisitos Previos

Antes de comenzar, cada miembro del equipo debe tener instalados los siguientes componentes:

| Componente   | Versión          | Propósito                  |
|--------------|------------------|----------------------------|
| JDK          | 25 o superior    | Compilar y ejecutar Java   |
| Maven        | 3.9+             | Gestión de dependencias    |
| PostgreSQL   | 17+              | Base de datos              |
| Git          | Última versión   | Control de versiones       |

## Instalación del Entorno

### 1. Verificar instalación de JDK

```bash
# Cerrar y abrir una nueva terminal
javac -version   # Debe mostrar: javac 25.x.x
java -version    # Debe mostrar: openjdk version 25.x.x
```

**IMPORTANTE:** Si no tienes `javac` ni `java`, procede con la instalación.

### 2. Instalación de JDK 25

**Descarga:**

```
https://download.oracle.com/java/25/archive/jdk-25.0.1_windows-x64_bin.exe
```

**Instalación:**

1. Ejecuta el instalador descargado
2. Sigue los pasos (siguiente → siguiente → instalar)
3. Ruta por defecto: `C:\Program Files\Java\jdk-25`

**Configurar variables de entorno:**

Método manual:

1. Presiona `Windows + X` → Sistema
2. Configuración avanzada del sistema → Variables de entorno
3. En Variables del sistema, haz clic en **Nueva**:
   - Variable: `JAVA_HOME`
   - Valor: `C:\Program Files\Java\jdk-25`
4. Busca la variable `Path` → Editar → Nueva:
   - Agrega: `%JAVA_HOME%\bin`
5. Haz clic en Aceptar en todas las ventanas

### 3. Instalación de Maven

**Descarga:**

1. Ve a: https://maven.apache.org/download.cgi
2. Descarga `apache-maven-3.9.x-bin.zip`

**Instalación:**

1. Extrae el zip en `C:\apache-maven-3.9.15`
2. Agrega al Path la carpeta `C:\apache-maven-3.9.15\bin`

**Verificar instalación:**

```bash
mvn --version
# Debe mostrar: Apache Maven 3.9.x y Java version: 25.x.x
```

### 4. Instalación de PostgreSQL

**Descarga:**

1. Ve a: https://www.postgresql.org/download/windows/
2. Descarga el instalador

**Instalación:**

1. Ejecuta el instalador
2. Durante la instalación:
   - Puerto: `5432` (por defecto)
   - Usuario: `postgres`
   - Contraseña: Elige una (ej: `admin123`)
3. Completa la instalación

**Crear la base de datos:**

Opción A: Usando psql

```sql
-- Conectar a PostgreSQL
psql -U postgres

-- Crear la base de datos
CREATE DATABASE edu_game_db;

-- Verificar
\l

-- Salir
\q
```

Opción B: Usando pgAdmin

1. Abre pgAdmin
2. Conecta al servidor
3. Click derecho en Databases → Create → Database
4. Nombre: `edu_game_db`
5. Owner: `postgres`
6. Guardar

**Verificar servicio:**

```powershell
Get-Service postgresql*
# Debe aparecer como "Running"
```

### 5. Instalación de Git

**Descarga:**

Ve a: https://git-scm.com/download/win

**Verificar:**

```bash
git --version
```

## Configuración del Proyecto

### 1. Clonar el repositorio

```bash
git clone https://github.com/TU_USUARIO/edukaplay.git
cd edukaplay
```

### 2. Crear archivo application.properties

**IMPORTANTE:** Este archivo NO se sube a Git por seguridad. Cada miembro debe crear el suyo.

Crea el archivo en: `src/main/resources/application.properties`

```properties
# PostgreSQL Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/edu_game_db
spring.datasource.username=postgres
spring.datasource.password=TU_CONTRASEÑA
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

## Estructura de Ramas y Flujo de Trabajo

### 1. Ramas principales

| Rama     | Propósito                         |
|----------|-----------------------------------|
| main     | Código estable en producción      |
| develop  | Integración de características    |

### 2. Ramas por desarrollador

Cada miembro del equipo crea su propia rama desde `develop` con el siguiente formato:

```
[NOMBRE]/feature-[DESCRIPCION]
```

**Ejemplos:**

- `axelAcosta/feature-models`
- `maria/feature-controllers`
- `juan/feature-frontend`
- `laura/feature-game-engine`

### 3. Flujo de trabajo diario

**Al inicio del día (actualizarse):**

```bash
# Cambiar a develop
git checkout develop

# Traer los últimos cambios
git pull origin develop

# Actualizar tu rama personal
git checkout axelAcosta/feature-models
git merge develop
```

**Durante el día (trabajar):**

```bash
# Hacer cambios en el código
git add .
git commit -m "feat: descripción clara del cambio"
git push origin axelAcosta/feature-models
```

**Al terminar una tarea (crear Pull Request):**

1. Subir los últimos cambios a tu rama
2. Ir a GitHub → Pull Requests → New Pull Request
3. Base: `develop` ← Compare: `axelAcosta/feature-models`
4. Agregar descripción de los cambios
5. Asignar al administrador del proyecto
6. Esperar aprobación

### 4. Diagrama del flujo de trabajo

```
main
  ↑
  └── Pull Request
        ↑
      develop (integración)
        ↑
        └── Pull Request
              ↑
            feature/axelAcosta-models (trabajo diario)
            feature/maria-controllers
            feature/juan-frontend
```

### 5. Comandos útiles para ramas

| Comando                                        | Propósito                              |
|------------------------------------------------|----------------------------------------|
| `git branch`                                   | Ver ramas locales                      |
| `git branch -a`                                | Ver todas las ramas (incluye remotas)  |
| `git checkout develop`                         | Cambiar a rama develop                 |
| `git checkout -b axelAcosta/feature-models`    | Crear y cambiar a nueva rama           |
| `git push -u origin axelAcosta/feature-models` | Subir rama por primera vez             |
| `git branch -d nombre-rama`                    | Eliminar rama local                    |
| `git push origin --delete nombre-rama`         | Eliminar rama remota                   |

### 6. Reglas del equipo

-  NUNCA trabajar directamente en `main` o `develop`
-  SIEMPRE crear rama desde `develop` con tu nombre
-  Siempre hacer Pull Request para integrar código
-  Siempre mantener tu rama actualizada con `develop`
-  Commits descriptivos (no "cambios", sí "feat: agregué modelo User")
-  No hacer merge de tu propia rama sin aprobación

## Ejecución de la Aplicación

### Desde PowerShell / Terminal:

```bash
# Navegar al proyecto
cd edukaplay

# Ejecutar la aplicación (Maven descarga dependencias automáticamente)
mvn spring-boot:run -DskipTests
```

### Desde IntelliJ IDEA:

1. Abrir el proyecto
2. Ejecutar la clase `EduGameApplication.java`
3. O usar el panel Maven → spring-boot:run

### Acceder a la aplicación:

```
http://localhost:8080
```

### Detener la aplicación:

Presiona `Ctrl + C` en la terminal
