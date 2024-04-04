pipeline {
    agent any
    tools {
        jdk 'JDK17'
        maven 'Maven'
        snyk 'Snyk'
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

        stage('SAST Scan') {
            steps {
               dir('SecOps') {
                  withCredentials([string(credentialsId: 'Snyk-Token', variable: 'SNYK_TOKEN')]) {
                       sh 'snyk code test --all-projects --json > snyk_sast_report.json'
                   }
                }
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
