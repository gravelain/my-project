pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = "docker.io"
        DOCKER_REPO_BACKEND = "thierrytemgoua98/my-project-backend"
        DOCKER_REPO_FRONTEND = "thierrytemgoua98/my-project-frontend"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Images') {
            parallel {
                stage('Build Backend Image') {
                    dir('backend') {
                        script {
                            def backendImage = "${env.DOCKER_REPO_BACKEND}:${env.BUILD_NUMBER}"
                            withDockerRegistry(credentialsId: 'docker-hub-credentials') {
                                sh "docker build -t ${backendImage} ."
                                sh "docker push ${backendImage}"
                            }
                        }
                    }
                }
                stage('Build Frontend Image') {
                    dir('frontend') {
                        script {
                            def frontendImage = "${env.DOCKER_REPO_FRONTEND}:${env.BUILD_NUMBER}"
                            withDockerRegistry(credentialsId: 'docker-hub-credentials') {
                                sh "docker build -t ${frontendImage} ."
                                sh "docker push ${frontendImage}"
                            }
                        }
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner'
                }
            }
        }
    }
    
    post {
        always {
            junit '**/test-results.xml'
        }
    }
}
