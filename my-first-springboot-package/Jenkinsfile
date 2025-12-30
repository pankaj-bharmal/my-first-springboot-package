pipeline {
    agent any

    tools {
        maven 'maven'   // Must match the Maven tool name in Jenkins global config
    }

    environment {
        DATE = "${new Date().format('yy.MM')}"
        TAG  = "${DATE}.${BUILD_NUMBER}"
        IMAGE_NAME = "localhost:5000/my-first-springboot-app"
        CONTAINER_NAME = "my-first-springboot-app"
    }

    stages {

        stage('Checkout Package Repo') {
            steps {
                echo "Package repo is already checked out"
            }
        }

        stage('Checkout App Source') {
            steps {
                dir('app-src') {
                    git branch: 'main',
                        url: 'https://github.com/pankaj-bharmal/my-first-springboot.git'
                }
            }
        }

        stage('Build Maven') {
            steps {
                dir('app-src') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    // Build Docker image using Dockerfile in package repo (root)
                    def image = docker.build("${IMAGE_NAME}:${TAG}", ".")
                    image.push()
                    image.push("latest")
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true

                docker pull ${IMAGE_NAME}:${TAG}

                docker run -d \
                  --name ${CONTAINER_NAME} \
                  -e TZ=Asia/Calcutta \
                  -p 9011:8080 \
                  --restart=always \
                  ${IMAGE_NAME}:${TAG}
                '''
            }
        }
    }

    post {
        success {
            echo "🚀 Deployment successful with tag ${TAG}"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
