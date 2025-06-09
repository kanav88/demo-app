pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kanav88/demo-app.git' // Replace with your own repo later
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Tests...'
            }
        }

        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: '**/*.jar', allowEmptyArchive: true
            }
        }
    }
}
