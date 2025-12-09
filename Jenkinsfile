pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "asfar946/devops-app:latest"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Asfar224/devops-project.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install flask pytest'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest tests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker rm -f devops-app || true"
                sh "docker run -d -p 5000:5000 --name devops-app $DOCKER_IMAGE"
            }
        }
    }
}
