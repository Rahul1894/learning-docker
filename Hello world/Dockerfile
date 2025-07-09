# 1. baseimage is our first layer of image 
FROM python:3.8-alpine  

# 2. Copy project files into the container, it create app folder inside Image and stores all files inside this folder
COPY . /app  


# 2. Set working directory in the container , now as all things are stored inside app folder , our working directory is app
WORKDIR /app

# 4. Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# 6. Command to run when the container starts
CMD ["python", "app.py"]
