# Práctica Ansible con 5 servidores Ubuntu en Docker Compose

Este repositorio contiene un laboratorio que utiliza Docker Compose para levantar cinco servidores Ubuntu con SSH y Ansible para configurar los servidores automáticamente.

## Archivos

```text
Dockerfile
ansible.cfg
docker-compose.yml
inventario.ini
playbook.yml
README.md
.gitignore
```

## Requisitos

- Docker y Docker Compose (por ejemplo, un GitHub Codespace con Docker disponible).
- Ansible y `sshpass` en la terminal desde la cual se administran los servidores.

> **Seguridad:** la contraseña `ansible` y `host_key_checking = False` se utilizan únicamente por exigencia de esta práctica. Los puertos SSH están publicados solo en `127.0.0.1` para no exponer el acceso por contraseña a otras redes. No reutilizar este esquema en producción.

## 1. Levantar los 5 contenedores

En el directorio donde está `docker-compose.yml`:

```bash
docker compose up -d --build
docker compose ps
```

Los cinco contenedores tienen SSH habilitado y el usuario `ansible`, contraseña `ansible`, con permisos sudo sin contraseña.

## 2. Instalar Ansible en Codespaces o Ubuntu/WSL

```bash
sudo apt-get update
sudo apt-get install -y ansible sshpass
ansible --version
```

Si trabajas desde Windows con WSL, debe estar configurado el acceso a Docker desde WSL. Una alternativa es ejecutar todo directamente en GitHub Codespaces.

## 3. Verificar acceso a los 5 servidores

```bash
ansible servidores -m ping
```

Deben aparecer los cinco hosts con `SUCCESS` y `"ping": "pong"`.

## 4. Ejecutar el playbook

```bash
ansible-playbook playbook.yml
```

El playbook realiza en **cada servidor**:

1. Actualización de la caché APT.
2. Creación del usuario `itla` con carpeta personal.
3. Creación de `/home/itla/app`.
4. Creación de `/home/itla/app/hola.txt` con el nombre del servidor.
5. Instalación de `cowsay` y `htop`.

## 5. Verificar la práctica

```bash
ansible servidores -m command -a 'id itla'
ansible servidores -m command -a 'cat /home/itla/app/hola.txt'
ansible servidores -m command -a 'dpkg-query -W cowsay htop'
```

Los comandos deben devolver resultados para los cinco servidores.

## 6. Evidencias recomendadas

Capturar las salidas de:

- `docker compose ps`, mostrando los 5 contenedores.
- `ansible servidores -m ping`, mostrando 5 respuestas.
- `ansible-playbook playbook.yml`, mostrando el resumen `PLAY RECAP` de 5 servidores sin errores.
- Verificación del usuario, el archivo y los dos paquetes.

## 7. Publicar el código en GitHub

```bash
git add .
git commit -m 'Practica Ansible con cinco servidores Ubuntu'
git push origin main
```

Entregar la URL pública del repositorio al profesor. El `README` y los archivos deben permanecer en la raíz del repositorio.
