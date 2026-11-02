
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
    }

    post {
        success {
            echo 'Image successfully pushed to Docker Hub!'
        }

        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
