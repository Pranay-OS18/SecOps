pipeline {
    agent any
    tools {
        jdk 'JDK17'
        maven 'Maven'
    }    
    environment {
        SNAP_REPO = 'secops-snapshot'
	RELEASE_REPO = 'secops-release'
	NEXUSIP = '43.204.147.217'
	NEXUSPORT = '8081'
        NEXUS_LOGIN = 'Nexus-Cred'
        DOCKER_VERSION = 'latest'
	registryCredential = 'ecr:ap-south-1:AWS-Cred'
        appRegistry = 'App-Registry'
        ecrRegistry = "ECR-Registry"
  
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Pranay-OS18/SecOps.git'
            }
        }
        stage('SAST Scan') {
            steps {
                snykSecurity(
                    snykInstallation: 'Snyk',
		    snykTokenId: 'Snyk-API-Token',
                    failOnIssues: false,
                    monitorProjectOnBuild: true,
                    additionalArguments: '--code --debug'
                )
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
                snykSecurity(
                    snykInstallation: 'Snyk',
		    snykTokenId: 'Snyk-API-Token',
                    failOnIssues: false,
                    monitorProjectOnBuild: true,
                    additionalArguments: '--sca --debug'
                )
            }
        }
        stage('Artifact To Nexus'){
            steps{
                nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                  groupId: 'QA',
                  version: "${env.BUILD_ID}",
                  repository: "${RELEASE_REPO}",
                  credentialsId: "${NEXUS_LOGIN}",
                  artifacts: [
                    [artifactId: 'secopsapp',
                     classifier: '',
                     file: 'target/database_service_project-0.0.2.jar',
                     type: 'jar']
                    ]
                )
            }
        }
	stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${appRegistry}:${BUILD_NUMBER}", ".")
                }
            }
        }
	stage('Image Scan') {
            steps {
                sh 'trivy image --format table -o scan-image-report.html ${appRegistry}:${BUILD_NUMBER}'
            }
        }    
        stage('Upload To ECR') {
            steps {
                script {
                    docker.withRegistry( ecrRegistry, registryCredential ) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}
     
