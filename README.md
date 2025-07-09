## Docker Quick Reference & Cheatsheet

A concise reference guide for common Docker commands and concepts. Use this README to quickly recall Docker operations when you need them.

---

### 1. Images vs Containers

* **Image**: A read-only template with instructions for creating a container. Like a snapshot or blueprint.
* **Container**: A runnable instance of an image. Contains everything needed to run the application.

---

### 2. Pulling & Listing Images

* **List local images:**

  ```bash
  docker images
  ```
* **Pull image from Docker Hub:**

  ```bash
  docker pull <image_name>:<tag>
  ```

  *Example:* `docker pull nginx:latest`

---

### 3. Running Containers

* **Run in foreground (interactive logs):**

  ```bash
  docker run -p <host_port>:<container_port> <image_name>
  ```
* **Run in background (detached mode):**

  ```bash
  docker run -d -p <host_port>:<container_port> <image_name>
  ```

  * `-d`: detached mode, frees your terminal
  * `-p host:container`: port mapping
* **Example:**

  ```bash
  docker run -d -p 8080:80 docker/getting-started
  ```

  Browse to `http://localhost:8080` to view the app.

---

### 4. Checking Running Containers

* **List running containers:**

  ```bash
  docker ps
  ```
* **List all containers (including stopped):**

  ```bash
  docker ps -a
  ```

---

### 5. Inspect & Logs

* **Fetch container logs:**

  ```bash
  docker logs [OPTIONS] <container_id_or_name>
  ```
* **Inspect container details:**

  ```bash
  docker inspect <container_id_or_name>
  ```

---

### 6. Stopping & Removing Containers

* **Stop a running container:**

  ```bash
  docker stop <container_id_or_name>
  ```
* **Remove a stopped container:**

  ```bash
  docker rm <container_id_or_name>
  ```
* **Stop & Remove in one command:**

  ```bash
  docker rm -f <container_id_or_name>
  ```

---

### 7. Cleaning Up Images & Containers

* **Remove unused images:**

  ```bash
  docker image prune
  ```
* **Remove all stopped containers & unused images:**

  ```bash
  docker system prune
  ```

---

### 8. Docker Run Options Cheat Sheet

| Option                        | Description                                 |
| ----------------------------- | ------------------------------------------- |
| `-d`                          | Run container in detached mode (background) |
| `-p host:container`           | Map host port to container port             |
| `-e KEY=VALUE`                | Set environment variable                    |
| `--name NAME`                 | Assign a name to the container              |
| `-v host_path:container_path` | Mount volume or bind-mount host directory   |
| `--rm`                        | Remove container after it stops             |

---

### 9. Useful Tips

* Use **container names** with `--name` for easier management:

  ```bash
  docker run -d --name webapp -p 80:80 nginx
  ```
* Redirect logs to a file:

  ```bash
  docker logs webapp &> webapp.log
  ```
* Find a container by name or partial ID:

  ```bash
  docker ps | grep webapp
  ```

---

*Keep this cheatsheet handy to streamline your Docker workflow!*
