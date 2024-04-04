pipeline {
    agent any
    tools {
        jdk 'JDK17'
        maven 'Maven'   
    }    
    environment {
        SNYK_TOKEN = credentials('Snyk-Token')    
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Pranay-OS18/SecOps.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('SCA Scan') {
            steps {
                dir('SecOps') {
                    withCredentials([string(credentialsId: 'Snyk-Token', variable: 'SNYK_TOKEN')]) {
                         sh 'snyk test --all-projects --json > snyk_sca_report.json'
                    } 
                }
            }
        }
    }
}    
