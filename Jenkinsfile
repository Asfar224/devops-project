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

        stage('Setup Virtual Environment & Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt || pip install flask pytest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest tests
                '''
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
                sh "docker run -d -p 5000:5000 --name devops-app --restart unless-stopped $DOCKER_IMAGE"
            }
        }
    }

    post {
        failure {
            echo "Build failed! Check the console output."
        }
        success {
            echo "Build succeeded! Docker container is running on port 5000."
        }
    }
}
