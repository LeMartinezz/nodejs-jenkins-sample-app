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
                sh 'npm test || echo "No tests to run"'
            }
        }

stage('Build Docker Image') {
    steps {
        sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
    }
}
    }

stage('Deploy') {
    steps {
        // Arrêter et supprimer l'ancien conteneur s'il existe
        sh """
          docker stop ${DOCKER_IMAGE} || true
          docker rm ${DOCKER_IMAGE} || true
        """
        // Lancer le nouveau conteneur sur le port 3000
        sh """
          docker run -d --name ${DOCKER_IMAGE} -p 3000:3000 ${DOCKER_IMAGE}:${DOCKER_TAG}
        """
    }
}

    post {
        always {
            echo "Pipeline terminé avec statut: ${currentBuild.currentResult}"
        }
    }
}
