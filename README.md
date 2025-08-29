# n8n Docker Setup

Una configuración simple de n8n usando Docker Compose para automatización de flujos de trabajo.

## ¿Qué es n8n?

n8n es una herramienta de automatización de flujos de trabajo que te permite conectar diferentes servicios y aplicaciones sin necesidad de código. Es una alternativa de código abierto a servicios como Zapier o Microsoft Power Automate.

## Requisitos

- Docker
- Docker Compose

## Configuración

### 1. Clonar el repositorio

```bash
git clone https://github.com/rjcd95/sf-n8n.git
cd sf-n8n
```

### 2. Levantar n8n

```bash
docker-compose up -d
```

### 3. Acceder a n8n

Una vez que el contenedor esté corriendo, puedes acceder a n8n en:

**URL:** http://localhost:5678

**Credenciales por defecto:**

- Usuario: `admin`
- Contraseña: `ASDF1234`

## Comandos útiles

### Iniciar n8n

```bash
docker-compose up -d
```

### Detener n8n

```bash
docker-compose down
```

### Ver logs

```bash
docker-compose logs -f n8n
```

### Reiniciar n8n

```bash
docker-compose restart n8n
```

## Configuración

Los datos de n8n se almacenan en `~/.n8n` en tu máquina local, por lo que tus flujos de trabajo se conservarán entre reinicios del contenedor.

### Cambiar credenciales

Para cambiar las credenciales de acceso, edita el archivo `docker-compose.yml` y modifica las variables:

```yaml
environment:
  - N8N_BASIC_AUTH_USER=tu_usuario
  - N8N_BASIC_AUTH_PASSWORD=tu_contraseña
```

Luego reinicia el contenedor:

```bash
docker-compose down
docker-compose up -d
```

## Primeros pasos

1. Accede a http://localhost:5678
2. Inicia sesión con las credenciales configuradas
3. Explora las plantillas disponibles o crea tu primer flujo de trabajo
4. Conecta tus servicios favoritos y automatiza tareas repetitivas

## Soporte

Para más información sobre n8n, visita la [documentación oficial](https://docs.n8n.io/).
