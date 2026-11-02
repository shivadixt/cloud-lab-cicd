
pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                checkout scm
                echo 'Code fetched successfully!'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                    -t shivadixit/cloud-lab-cicd:${BUILD_NUMBER} \
                    -t shivadixit/cloud-lab-cicd:latest .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh '''
                    docker push shivadixit/cloud-lab-cicd:${BUILD_NUMBER}
                    docker push shivadixit/cloud-lab-cicd:latest
                '''
            }
        }

        stage('Deploy Locally') {
            steps {
                sh '''
                    docker pull shivadixit/cloud-lab-cicd:${BUILD_NUMBER}

                    docker rm -f cloud-lab-container || true

                    docker run -d \
                    --name cloud-lab-container \
                    --restart unless-stopped \
                    -p 8081:80 \
                    shivadixit/cloud-lab-cicd:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
            echo 'Application: http://localhost:8081'
        }

        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
