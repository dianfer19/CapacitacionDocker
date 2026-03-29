Crack, así está bien para demo local 👍

Para sacar el **usuario y contraseña inicial** de GitLab:

## Usuario

```text
root
```

## Contraseña inicial

Ejecuta esto:

```bash
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

También puedes ver el archivo completo:

```bash
docker exec -it gitlab cat /etc/gitlab/initial_root_password
```

Te va a salir algo como:

```text
Password: xxxxxxxxxxxxxxxxx
```

---

# Importante

Ese archivo de contraseña inicial **no dura para siempre**. GitLab indica que puede eliminarse automáticamente después de la primera reconfiguración o tras un tiempo, así que conviene verla apenas levanta.

---

# Si no aparece

Puedes resetear la contraseña de `root` así:

```bash
docker exec -it gitlab gitlab-rake "gitlab:password:reset[root]"
```

---

# Cómo lo explicas en clase

> “GitLab crea el usuario administrador inicial `root` y genera una contraseña temporal que podemos consultar dentro del contenedor.”

---

# Tip fino

Para tu demo, entra así:

```text
http://localhost
```

usuario:

```text
root
```
https://docs.gitlab.com/18.10/administration/auth/ldap/?tab=Docker


