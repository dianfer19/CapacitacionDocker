
# 🐳 🧠 Comandos básicos de Docker

## 🚀 Contenedores

```bash
docker run <imagen>                # Crear y ejecutar un contenedor
docker run -d <imagen>             # Ejecutar en segundo plano
docker run -p 8080:80 <imagen>     # Mapear puertos
docker run --name mi_app <imagen>  # Asignar nombre
```

---

## 👀 Inspección

```bash
docker ps                          # Contenedores en ejecución
docker ps -a                       # Todos los contenedores
docker logs <id|name>              # Ver logs
docker inspect <id|name>           # Información detallada
```

---

## 🔄 Control de contenedores

```bash
docker stop <id|name>              # Detener
docker start <id|name>             # Iniciar
docker restart <id|name>           # Reiniciar
docker rm <id|name>                # Eliminar
docker rm -f <id|name>             # Forzar eliminación
```

---

## 🧠 Imágenes

```bash
docker images                      # Listar imágenes
docker pull <imagen>               # Descargar imagen
docker build -t mi_app .           # Construir imagen
docker rmi <imagen>                # Eliminar imagen
```

---

## 🐚 Acceso a contenedor

```bash
docker exec -it <id|name> sh       # Entrar al contenedor
docker exec -it <id|name> bash     # (si tiene bash)
```

---

## 💾 Volúmenes

```bash
docker volume ls                   # Listar volúmenes
docker volume create mi_vol        # Crear volumen
docker volume rm mi_vol            # Eliminar volumen
```

---

## 🔌 Redes

```bash
docker network ls                  # Listar redes
docker network create mi_red       # Crear red
docker network rm mi_red           # Eliminar red
```

---

## 🧩 Docker Compose (moderno)

```bash
docker compose up                  # Levantar servicios
docker compose up -d               # En segundo plano
docker compose down                # Detener servicios
```

---

# 💀 Bonus (limpieza rápida)

```bash
docker container prune             # Eliminar contenedores detenidos
docker volume prune                # Eliminar volúmenes no usados
docker system prune -a             # Limpieza total
```

