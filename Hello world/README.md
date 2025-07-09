# 🐳 Flask App Dockerization - README

This README explains how to containerize a simple Flask web app using Docker. It includes the required files, Dockerfile breakdown, commands, and usage steps.

---

## 📁 Project Structure

```
/your-project-folder
├── app.py
├── requirements.txt
└── Dockerfile
```

---

## 📄 app.py

A simple Flask application:

```python
from flask import Flask
import os
app = Flask(__name__)

@app.route("/", methods=['GET'])
def home():
    return "Hello World"

if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0", port=5000)
```

---

## 📄 requirements.txt

Include this to specify Python dependencies:

```
flask
```

---

## 🐳 Dockerfile

```Dockerfile
# 1. Use Python base image
FROM python:3.8-alpine

# 2. Set working directory
WORKDIR /app

# 3. Copy all files to /app inside the image
COPY . /app

# 4. Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# 5. Define default command to run the app
CMD ["python", "app.py"]
```

---

## 🛠️ Docker Commands

### ✅ Build the Docker Image

```bash
docker build -t welcome-app .
```

### ✅ Run the Container

```bash
docker run -p 5000:5000 welcome-app
```

### ✅ View the App

Visit in browser:

```
http://localhost:5000
```

You should see:

```
Hello World
```

---

## 📚 Dockerfile Instruction Explanation

| Line      | Explanation                                                     |
| --------- | --------------------------------------------------------------- |
| `FROM`    | Uses Python 3.8 Alpine as base image (lightweight)              |
| `WORKDIR` | Sets `/app` as the working directory inside the container       |
| `COPY`    | Copies all files from current host dir into `/app` in the image |
| `RUN`     | Installs required Python packages from `requirements.txt`       |
| `CMD`     | Tells Docker how to start your app when container runs          |

---

## 🧼 Optional Cleanup

### Stop the container (in another terminal):

```bash
docker ps  # Find container ID
docker stop <container_id>
```

### Remove the image:

```bash
docker rmi welcome-app
```

---

## 🧠 Notes

* `0.0.0.0` makes the app accessible outside the container.
* `-p 5000:5000` maps host port 5000 to container port 5000.

---

Happy Dockering! 🚀
