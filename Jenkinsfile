pipeline {
    agent any
    tools {
        jdk 'JDK17'
        maven 'Maven'
        snyk 'Snyk'
        docker 'Docker'
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
    }
}
