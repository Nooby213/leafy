pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = credentials('docker') // Docker Hub에 대한 자격 증명 ID
        DOCKER_REGISTRY = 'your-docker-registry' // Docker Hub 또는 개인 레지스트리
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Git 저장소에서 소스코드를 가져옵니다.
                    checkout scm
                }
            }
        }

        stage('Build leafy-backend') {
            steps {
                script {
                    dir('leafy-backend') {
                        // leafy-backend 폴더에서 Dockerfile을 사용하여 이미지를 빌드합니다.
                        sh 'docker build -t leafy-backend:latest .'
                    }
                }
            }
        }

        stage('Build leafy-frontend') {
            steps {
                script {
                    dir('leafy-frontend') {
                        // leafy-frontend 폴더에서 Dockerfile을 사용하여 이미지를 빌드합니다.
                        sh 'docker build -t leafy-frontend:latest .'
                    }
                }
            }
        }

        stage('Build leafy-postgresql') {
            steps {
                script {
                    dir('leafy-postgresql') {
                        // leafy-postgresql 폴더에서 Dockerfile을 사용하여 이미지를 빌드합니다.
                        sh 'docker build -t leafy-postgresql:latest .'
                    }
                }
            }
        }


        stage('Deploy') {
            steps {
                script {
                    // Docker Compose를 사용하여 애플리케이션을 배포합니다.
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            // 빌드 작업이 끝난 후 항상 실행될 작업
            cleanWs() // 워크스페이스 정리
        }
        success {
            echo 'Deployment was successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
