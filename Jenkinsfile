pipeline {
    agent any
    tools{
        nodejs 'nodejs'
    }
    environment {
        SCANNER_HOME= tool 'sonar-scanner' 
    }
    stages {
        stage('git checkout') {
            steps {
                git 'https://github.com/sunnyramagiri/3tire.git'
            }
        }
        stage('install depandancy') {
            steps {
                sh 'npm install'
            }
        }
        stage('unit testing') {
            steps {
                sh 'npm test'
            }
        }
        stage('trivy fs scan') {
            steps {
                sh 'trivy fs --formate table -o -report.html'
            }
        }
        stage('sonarQube') {
            steps {
                sh ' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey= Campground -Dsonar.projectName=Campground'
            }
        }
        stage('docker build & tag') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh 'docker build -t sunnyramagiri/camp:latest' 
                    }
                }
            }
        }
        stage('docker scane') {
            steps {
                sh ' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey= Campground -Dsonar.projectName=Campground'
            }
        }
        stage('docker push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh 'docke rpush sunnyramagiri/camp:latest' 
                    }
                }
            }
        }
        stage('docker deploy to Dev') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh 'docker run -d -p 3000:3000 sunnyramagiri/camp:latest' 
                    }
                }
            }
        }
    }
}
