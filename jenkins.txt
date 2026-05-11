pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'JDK25'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/justaguy1337/prog6.git'
            }
        }

        stage('Build') {
            steps {
                dir('demo') {
                    bat 'mvn clean package'
                }
            }
        }

        stage('Test') {
            steps {
                dir('demo') {
                    bat 'mvn test'
                }
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true,
                  testResults: '**/target/surefire-reports/*.xml'
        }
    }
}