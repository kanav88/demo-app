pipeline {
    agent any

    environment {
        NEXUS_URL = "http://192.168.49.2:30081/repository/maven-releases/"
        NEXUS_CREDENTIALS_ID = "nexus-admin-credentials"
    }

    tools {
        maven 'maven-3'
        jdk 'jdk-17'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Create Artifact') {
            steps {
                // Create a dummy JAR file as artifact
                sh '''
                mkdir -p target
                echo "Demo artifact from Jenkins pipeline" > target/demo.txt
                jar cf target/demo.jar -C target demo.txt
                '''
            }
        }
        stage('Publish to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.NEXUS_CREDENTIALS_ID, usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    sh """
                    mvn deploy:deploy-file \
                        -Durl=${NEXUS_URL} \
                        -DrepositoryId=nexus \
                        -Dfile=target/demo.jar \
                        -DgroupId=com.example.demo \
                        -DartifactId=demo-artifact \
                        -Dversion=1.0.0 \
                        -Dpackaging=jar \
                        -DgeneratePom=true \
                        -Dusername=$NEXUS_USER \
                        -Dpassword=$NEXUS_PASS
                    """
                }
            }
        }
    }
}
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
