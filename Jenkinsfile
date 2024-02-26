pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs '21.6.2'
        maven 'Maven3.9.6'
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/claudedevops/DevSecOps-Project.git'
            }
        }
        stage("Sonarqube Analysis") {
            environment {
                SONAR_URL = 'http://3.101.138.146:9000'
            }
            steps {
                withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_TOKEN')]) {
                    sh 'mvn sonar:sonar - Dsonar.login=${SONAR_TOKEN -Dsonar.host.url=${SONAR_URL}'
                }
            }
        }
        stage("quality gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
    }
}
