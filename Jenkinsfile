pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'abdullahdiallo'  // Nom de ton Docker registry
        IMAGE_NAME = 'gestion-etablissement'
    }

    tools {
        maven 'Maven'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/AbdullahDiallo/ExamenDevops.git'
            }
        }

        stage('Build Backend (Spring Boot)') {
            steps {
                dir('backend') {  // Change de répertoire pour aller dans le backend
                    sh 'mvn clean package'  // Construire le backend avec Maven
                }
            }
        }

        stage('Build Frontend (Angular)') {
            steps {
                dir('Gestion2-main') {  // Change de répertoire pour aller dans le frontend
                    sh 'npm install'  // Installer les dépendances Angular
                    sh 'ng build --prod'  // Construire le frontend
                }
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
