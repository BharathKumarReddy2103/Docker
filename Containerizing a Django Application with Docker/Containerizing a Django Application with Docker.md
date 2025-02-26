**Containerizing a Django Web Application Using Docker**

**Overview**

This repository provides a step-by-step guide on how to **containerize a Django web application** using **Docker**. The guide covers:

•	**Setting up a Django application**

•	**Writing a Dockerfile for containerization**

•	**Building and running the container**

•	**Common troubleshooting tips**

By following this guide, you will learn how to deploy a Django application as a **Docker container**, making it easy to run across different environments.

---

**Prerequisites**

Before proceeding, ensure you have:

•	**Python 3+ installed** on your local machine.

•	**Docker installed** on your system.

•	**Basic understanding** of Django applications.

---

**1. Setting Up a Django Application**

**Step 1: Install Django**

First, ensure that Python and pip are installed, then install Django:

```sh
pip install django
```

Verify the installation:

```sh
python -c "import django; print(django.get_version())"
```

**Step 2: Create a Django Project**

Run the following command to create a new Django project:

```sh
django-admin startproject myproject
cd myproject
```

**Step 3: Create a Django App**

Inside the project directory, create an app:

```sh
python manage.py startapp myapp
```

This command generates essential Django files such as views.py, models.py, admin.py, and urls.py.

**Step 4: Define a Simple View**

Modify myapp/views.py to create a basic Django view:

```sh
from django.http import HttpResponse

def home(request):
    return HttpResponse("Hello, Dockerized Django!")
```

Map this view in myapp/urls.py:

```sh
from django.urls import path
from .views import home

urlpatterns = [
    path('', home),
]
```

Then, include this app in your myproject/urls.py:

```sh
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),
]
```

**Step 5: Define Dependencies**

Create a requirements.txt file to list dependencies:

```sh
django
```

Install the dependencies:

```sh
pip install -r requirements.txt
```

---

**2. Writing a Dockerfile**

Create a Dockerfile in the project root directory:

```sh
# Use an official Python runtime as a parent image
FROM python:3.9

# Set the working directory in the container
WORKDIR /app

# Copy project files to the container
COPY . /app

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Expose the application port
EXPOSE 8000

# Run the Django development server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

**Dockerfile Breakdown**

**1.	Base Image** (FROM python:3.9) → Uses an official Python image.

**2.	Work Directory** (WORKDIR /app) → Defines the working directory.

**3.	Copying Files** (COPY . /app) → Copies all files into the container.

**4.	Installing Dependencies** (RUN pip install) → Installs required packages.

**5.	Exposing Port** (EXPOSE 8000) → Opens port 8000 for access.

**6.	Running the Application** (CMD ["python", "manage.py", "runserver"]) → Starts the Django server.

---

**3. Building and Running the Docker Container**

**Step 1: Build the Docker Image**

Run the following command to build the Docker image:

```sh
docker build -t django-app .
```

**Step 2: Run the Docker Container**

Start the container with:

```sh
docker run -p 8000:8000 django-app
```

**Step 3: Access the Application**

Open your web browser and visit:

```sh
http://localhost:8000
```

If running on **AWS EC2**, replace localhost with the **EC2 instance public IP**.

---

**4. Troubleshooting Common Issues**

**Issue 1: Application Not Loading**

Ensure your EC2 instance allows **port 8000** by updating inbound security rules.

**Issue 2: Container Exits Immediately**

Run in **interactive mode**:

```sh
docker run -it django-app
``

Check logs:

```sh
docker logs <container_id>
```

**Issue 3: Modifications Not Reflecting**

Rebuild and restart the container:

```sh
docker build -t django-app .
docker rm -f $(docker ps -aq)
docker run -p 8000:8000 django-app
```

---

**5. Next Steps**

Now that you've containerized a Django application, explore:

•	**Docker Networking** → How containers communicate internally.

•	**Multi-Stage Docker Builds** → Optimizing image size.

•	**CI/CD with Docker** → Automating deployments.

---

**Conclusion**

This guide covers **everything you need to know** about containerizing a Django application using Docker. Try it out, experiment with different settings, and improve your DevOps skills.
