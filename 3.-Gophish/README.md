## 1. Requisitos

- Docker
- Docker Compose

---

## 2. Estructura del proyecto

```text
.
├─ docker-compose.yml
├─ .env.example
├─ README.md
└─ (volumen docker) gophish-data  # datos persistentes
````

---

## 3. Configuración rápida

### 3.1. Crear el archivo `.env`

A partir del ejemplo:

```bash
cp .env.example .env
```

Ejemplo de contenido de `.env`:

```env
# Zona horaria del contenedor
TZ=America/Guayaquil

# Opcional: referencia para tu configuración SMTP
# (Gophish no las usa automáticamente, solo las guardo aquí
# para no ponerlas en claro en el código ni en capturas)
SMTP_HOST=mail.iconix.ec
SMTP_PORT=465
SMTP_USERNAME=tu-correo@dominio.ec
SMTP_PASSWORD=tu-super-clave
#Variable de entorno para la clave de administrador se inicialice con una clave en particular
GOPHISH_INITIAL_ADMIN_PASSWORD=tu-clave
```

> 💡 Nota:
> Las variables `SMTP_*` **no** configuran Gophish automáticamente.
> La configuración SMTP real se hace dentro del panel web de Gophish (Sending Profiles).
> El `.env` sirve para tener las credenciales fuera del código y no subirlas al repo.

---

### 3.2. Levantar el entorno

```bash
docker compose up -d
```

Para ver logs:

```bash
docker compose logs -f
```

En los logs iniciales verás algo como:

```txt
Please login with the username admin and the password XXXXXXXXXX
```

Guarda esa contraseña: es la del usuario `admin` del panel.

---

## 4. Acceso a Gophish

Con el `docker-compose.yml` incluido, los servicios quedan así:

* **Panel de administración**
  `https://127.0.0.1:3333`

* **Servidor de phishing / landing pages**
  `http://127.0.0.1:8080`

> 🔐 Ambos están enlazados solo a `127.0.0.1` (localhost),
> es decir, solo accesibles desde la misma máquina.
> Perfecto para laboratorio y para evitar exponer Gophish a Internet.

---

## 5. Explicación del `docker-compose.yml`

Fragmento principal:

```yaml
services:
  gophish:
    image: gophish/gophish:latest
    container_name: gophish
    restart: unless-stopped
    ports:
      - "127.0.0.1:3333:3333"  # Panel admin (TLS)
      - "127.0.0.1:8080:80"    # Phish server (HTTP)
    env_file:
      - .env
    volumes:
      - gophish-data:/opt/gophish
    security_opt:
      - no-new-privileges:true
    tmpfs:
      - /tmp
    networks:
      - gophish_net

networks:
  gophish_net:
    driver: bridge

volumes:
  gophish-data:
```

### Puertos

* `127.0.0.1:3333:3333`

    * El puerto 3333 del **host** se mapea al 3333 del **contenedor**.
    * Solo escucha en `127.0.0.1` → no expuesto hacia fuera.

* `127.0.0.1:8080:80`

    * El puerto 8080 del **host** se mapea al 80 del **contenedor** (phish_server).
    * También solo en `127.0.0.1`.

### Volumen `gophish-data`

```yaml
volumes:
  - gophish-data:/opt/gophish
```

* Guarda de forma persistente:

    * `config.json`
    * `gophish.db` (SQLite)
    * plantillas
    * landing pages
* Aunque borres el contenedor, los datos siguen ahí.

> 💡 En Windows / WSL es más cómodo usar **named volumes**
> (como en este ejemplo) que montar carpetas locales, para evitar permisos raros.

---

## 6. Flujo básico de uso

1. Acceder al panel admin:
   `https://127.0.0.1:3333`
   User: `admin`
   Pass: la que sale en los logs.

2. Configurar un **Sending Profile** con tu SMTP de pruebas.

3. Crear:

    * un **Email Template** con enlace `{{.URL}}`
    * una **Landing Page** (opción *Capture Submitted Data* activada si es lab)
    * un **Group** con tus propios correos

4. Crear una **Campaign**:

    * URL: `http://127.0.0.1:8080`
    * Email Template + Landing Page + Sending Profile + Group

5. Lanzar la campaña y revisar:

    * `Email Sent`
    * `Email Opened`
    * `Clicked Link`
    * `Submitted Data`

---

## 7. Producción / siguiente paso

Este repo está pensado como **laboratorio local**.
Para un entorno institucional (servidor de Aduana, etc.) se recomienda:

* Usar un servidor Linux dedicado
* Poner Gophish detrás de un **reverse proxy** (Nginx / Nginx Proxy Manager)
* Exponer solo:

    * Admin por VPN / intranet
    * Phish server con dominio dedicado de pruebas
* Contar siempre con:

    * autorización formal
    * documento de alcance
    * políticas de seguridad claras

---

## 8. Aviso ético

> Este proyecto se utiliza únicamente para fines educativos y de concienciación en ciberseguridad.
> El uso indebido de estas técnicas o herramientas puede ser ilegal.
> Cada implementación real debe contar con autorización explícita de la organización responsable.