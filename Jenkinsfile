pipeline {
    agent any

   environment {
    DOCKER_REGISTRY = 'docker.io/abdullahdiallo'  
    IMAGE_NAME = 'gestion-etablissement'
}


    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/AbdullahDiallo/ExamenDevops.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Quality Check') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_REGISTRY/$IMAGE_NAME:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_REGISTRY/$IMAGE_NAME:latest'
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Deploy to Kubernetes (Prod)') {
            when {
                branch 'main'
            }
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
