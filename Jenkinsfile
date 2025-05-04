pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'forex'
        DOCKER_TAG = 'latest'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '''
                echo "Building image: ${DOCKER_IMAGE}:${DOCKER_TAG}"
                docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                '''
            }
        }

        stage('Verify Image Build') {
            steps {
                sh '''
                echo "Checking built images..."
                docker images | grep ${DOCKER_IMAGE}
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} $DOCKER_USER/${DOCKER_IMAGE}:${DOCKER_TAG}
                    docker push $DOCKER_USER/${DOCKER_IMAGE}:${DOCKER_TAG}
                    '''
                }
            }
        }
    }
}
