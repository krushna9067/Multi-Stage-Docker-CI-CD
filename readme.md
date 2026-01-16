
# 🚀 Multi-Stage Docker CI/CD with Jenkins on AWS EC2

## 📌 Project Overview
This project demonstrates how to build a fully automated **CI/CD pipeline** using **Jenkins** and **Docker** on an **AWS EC2 instance**.  

The project uses a **multi-stage Dockerfile** to optimize Docker image size and performance. The pipeline **builds the Docker image, runs tests, and deploys the container only if tests pass**.  

This project is beginner-friendly and ideal for DevOps freshers.

---

## 🏗️ Architecture
```

Developer → GitHub → Jenkins → Docker → AWS EC2

````

---

## 🛠️ Technologies Used
- **AWS EC2** – Hosting Python application and Jenkins  
- **Jenkins** – CI/CD automation  
- **Docker** – Containerization and multi-stage build  
- **Git & GitHub** – Version control  
- **Python (Flask)** – Application  
- **Pytest** – Automated testing  

---

## ⚙️ Setup Steps

### Step 1: Create Python Application
- `app.py`:
```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Multi-Stage Docker CI/CD with Jenkins"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
````

* `requirements.txt`:

```
flask==2.3.2
pytest
```

* `test_app.py`:

```python
from app import app

def test_home():
    response = app.test_client().get('/')
    assert response.status_code == 200
    assert b"Multi-Stage Docker CI/CD" in response.data
```

---

### Step 2: Create Multi-Stage Dockerfile

```Dockerfile
# Stage 1: Build
FROM python:3.10-slim as builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user -r requirements.txt
COPY . .

# Stage 2: Production
FROM python:3.10-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY --from=builder /app /app
ENV PATH=/root/.local/bin:$PATH
CMD ["python", "app.py"]
```

---

### Step 3: Push Code to GitHub

```bash
git init
git add .
git commit -m "Initial commit for multi-stage Docker CI/CD"
git remote add origin https://github.com/krushna9067/Multi-Stage-Docker-CI-CD.git
git push -u origin master
```

---

### Step 4: Configure Jenkins

1. Open Jenkins: `http://<EC2-PUBLIC-IP>:8080`
2. Install **Docker Pipeline plugin**
3. Create **New Item → Pipeline → OK**

---

### Step 5: Create Jenkinsfile

```groovy
pipeline {
    agent any
    environment {
        IMAGE_NAME = "multi-stage-app"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/krushna9067/Multi-Stage-Docker-CI-CD.git'
            }
        }
        stage('Build & Test') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'pytest'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop multi-stage-app || true
                docker rm multi-stage-app || true
                docker run -d -p 5000:5000 --name multi-stage-app multi-stage-app
                '''
            }
        }
    }
}
```

---

### Step 6: Run Jenkins Pipeline

* Click **Build Now** → Watch pipeline stages:

  * Checkout → Build & Test → Docker Build → Run Container
* Only if tests pass, the app will run on EC2

---

### Step 7: Access Application

```
http://<EC2-PUBLIC-IP>:5000
```

You should see:

```
Multi-Stage Docker CI/CD with Jenkins
```
---

## ✅ Outcome

* Optimized Docker image with **multi-stage build**
* CI/CD pipeline with Jenkins fully automated
* Tests run automatically before deployment
* Python Docker app deployed on AWS EC2

---

## 👨‍💻 Author

**Krushna Nangare**
DevOps Engineer (Fresher)
Skills: AWS | Docker | Jenkins | Kubernetes | Terraform | Git

---

## ⭐ Conclusion

This project demonstrates **real-world CI/CD automation** using Jenkins and Docker with AWS EC2, helping freshers gain **hands-on DevOps experience**

Do you want me to do that next?
```
