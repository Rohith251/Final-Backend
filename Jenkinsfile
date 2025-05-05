pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t rohith0702/forex:latest .'
            }
        }

      stage('Push to Docker Hub') {
    steps {
        sh '''
            echo "Rohith@0702" | docker login -u rohith0702 --password-stdin
            docker push rohith0702/forex:latest
        '''
    }
}
    }
}
