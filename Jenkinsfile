pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'abdullahdiallo'
    }

    tools {
        maven 'Maven'
        nodejs 'Node'
        dockerTool 'Docker'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/AbdullahDiallo/ExamenDevops.git'
            }
        }

        stage('Install Node.js & Angular CLI') {
            steps {
                script {
                    sh 'npm install -g @angular/cli'
                }
            }
        }

        stage('Build Backend Services') {
            steps {
                script {
                    def services = ['students', 'professeurs', 'cours', 'classes', 'timetable']
                    for (service in services) {
                        dir("backend/${service}") {
                            sh "mvn clean package"
                        }
                    }
                }
            }
        }

        stage('Build Frontend (Angular)') {
            steps {
                dir('Gestion2-main') {  
                    sh 'npm install'  
                    sh 'ng build --configuration=production'  
                }
            }
        }

        stage('Pull Docker Images from Docker Hub') {
            steps {
                script {
                    def services = ['students', 'professeurs', 'cours', 'classes', 'timetable']
                    for (service in services) {
                        sh "docker pull $DOCKER_REGISTRY/${service}:latest"
                    }
                }
                sh "docker pull $DOCKER_REGISTRY/frontend:latest"
            }
        }

        stage('Deploy to Kubernetes') {
            when {
                branch 'main'
            }
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
