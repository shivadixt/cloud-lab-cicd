
pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                echo 'Source code fetched successfully!'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t cloud-lab-site:latest .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                sh 'docker images cloud-lab-site'
            }
        }
    }
}
