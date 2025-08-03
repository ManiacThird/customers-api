pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:$PATH"
        IMAGE_NAME = "customers-api"
        CONTAINER_NAME = "customers-api-container"
        APP_PORT = "5002"
        DOCKER_PORT = "80"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ManiacThird/customers-api.git'
            }
        }

        stage('Check .NET Version') {
            steps {
                sh 'dotnet --version'
            }
        }

        stage('Clean Old Containers/Images') {
            steps {
                sh '''
                    # Detener y eliminar contenedor si existe
                    if [ "$(docker ps -aq -f name=$CONTAINER_NAME)" ]; then
                        docker stop $CONTAINER_NAME || true
                        docker rm $CONTAINER_NAME || true
                    fi

                    # Eliminar imágenes sin tag (dangling)
                    docker image prune -f
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Docker') {
            steps {
                sh 'docker run -d --name $CONTAINER_NAME -p $APP_PORT:$DOCKER_PORT $IMAGE_NAME'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
