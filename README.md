# Proyecto MSSQL Server 2017 con Docker Compose

Este proyecto proporciona un entorno contenerizado para ejecutar Microsoft SQL Server 2017 (Express Edition) utilizando Docker Compose. Está configurado para persistir los datos en una carpeta local y permitir una fácil conexión desde clientes SQL externos.

## 📋 Requisitos Previos

Asegúrate de tener instalados los siguientes componentes en tu sistema:

- [Docker Engine](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## 🚀 Estructura del Proyecto

El proyecto consta de los siguientes archivos y carpetas:

- `docker-compose.yml`: Define el servicio `mssql`, la imagen a utilizar, puertos y volúmenes.
- `.env`: Archivo de configuración para variables de entorno sensibles (como la contraseña).
- `data/`: Directorio local donde se almacenarán los archivos de base de datos (`.mdf`, `.ldf`).
- `.gitignore`: Configuración para evitar subir archivos sensibles o datos pesados al repositorio.

## ⚙️ Configuración

### 1. Variables de Entorno
El archivo `.env` contiene la contraseña del usuario administrador (`sa`). 

**Importante:** Antes de desplegar en un entorno compartido o de producción, cambia la contraseña por defecto.

Archivo `.env`:
```ini
MSSQL_SA_PASSWORD=TuPasswordFuerte!123
```
*Nota: SQL Server requiere contraseñas complejas (mínimo 8 caracteres, mayúsculas, minúsculas, números y símbolos).*

## 🛠️ Uso

### Iniciar el servicio
Para levantar el contenedor en segundo plano:

```bash
docker-compose up -d
```

### Ver el estado
Para comprobar si el contenedor está corriendo:

```bash
docker ps
```

### Ver logs
Si el contenedor se detiene o tienes problemas de conexión, revisa los logs:

```bash
docker-compose logs -f
```

### Detener el servicio
Para detener y eliminar el contenedor (los datos se conservarán):

```bash
docker-compose down
```

## 🔌 Conexión a la Base de Datos

Puedes conectarte utilizando cualquier cliente SQL (como Azure Data Studio, DBeaver, SSMS o `sqlcmd`) con los siguientes parámetros:

| Parámetro | Valor |
|-----------|-------|
| **Host/Servidor** | `localhost` (o `127.0.0.1`) |
| **Puerto** | `1433` |
| **Usuario** | `sa` |
| **Contraseña** | La definida en tu archivo `.env` |
| **Método de Autenticación** | SQL Server Authentication |

## 💾 Persistencia de Datos

La configuración de volúmenes en `docker-compose.yml` mapea el directorio local `./data` al directorio interno del contenedor `/var/opt/mssql/data`.

```yaml
volumes:
  - ./data:/var/opt/mssql/data
```

Esto significa que:
1. Todos los archivos de base de datos (`.mdf`, `.ldf`) se guardarán en tu carpeta `data/` local.
2. Si eliminas el contenedor, **tus bases de datos no se perderán**.
3. Para hacer un backup físico, simplemente puedes copiar el contenido de la carpeta `data/` (con el contenedor detenido).

### Limpieza total
Si deseas reiniciar todo desde cero y **borrar todas las bases de datos**:

1. Detén el contenedor: `docker-compose down`
2. Borra el contenido de la carpeta de datos: `rm -rf data/*`
3. Inicia de nuevo: `docker-compose up -d`

## ℹ️ Notas Adicionales

- **Versión de SQL Server:** Se utiliza la versión **2017** (`mcr.microsoft.com/mssql/server:2017-latest`) ya que es la primera versión con soporte oficial para contenedores Linux. SQL Server 2012 no es compatible con este entorno.
- **Permisos:** El contenedor se ejecuta como `root` para evitar problemas de permisos de escritura en el volumen montado en sistemas macOS/Linux.