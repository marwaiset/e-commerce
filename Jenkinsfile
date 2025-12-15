
pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'maven3.6.3'
    }

    environment {
        TOMCAT_URL = 'http://localhost:8080'
        TOMCAT_CREDENTIALS = 'tomcat'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/marwaiset/e-commerce.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [
                    tomcat9(
                        credentialsId: "${TOMCAT_CREDENTIALS}",
                        path: '',
                        url: "${TOMCAT_URL}"
                    )
                ],
                contextPath: 'e-commerce',
                war: 'target/*.war'
            }
        }
    }

    post {
        success {
            echo '✅ Déploiement réussi sur Tomcat'
        }
        failure {
            echo '❌ Échec de la pipeline'
        }
    }
}
