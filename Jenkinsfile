pipeline {
    agent any
    tools {
        jdk 'JDK17'
        maven 'Maven'
        snykInstallation 'Snyk'
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
                  withCredentials([string(snykTokenId: 'Snyk-Token')]) {
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
         
        stage('SCA Scan') {
            steps {
                dir('SecOps') {
                    withCredentials([string(snykTokenID: 'Snyk-Token')]) {
                         sh 'snyk test --all-projects --json > snyk_sca_report.json'
                    } 
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
    }
}    
