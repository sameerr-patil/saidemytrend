pipeline {
    agent any

    tools {
        
        maven 'M2-home'     
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean deploy'
            }
        }

        stage('SonarQube analysis') {
            steps {
                script {
                    def scannerHome = tool 'sam-sonarqube-scanner'  
                    withSonarQubeEnv('sam-sonarqube--server') {       
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}

