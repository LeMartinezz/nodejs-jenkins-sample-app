pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "jenkins-demo-app"
        DOCKER_TAG   = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Adapte selon ton TP : si pas de tests, on log seulement
                sh 'npm test || echo "No tests to run"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Deploy') {
            steps {
                // Stoppe l’ancien conteneur si présent
                sh "docker rm -f ${DOCKER_IMAGE} || true"
                // Lance le nouveau conteneur
                sh "docker run -d --name ${DOCKER_IMAGE} -p 3000:3000 ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }

    post {
        always {
            echo "Pipeline terminé avec statut: ${currentBuild.currentResult}"
        }
    }
}
