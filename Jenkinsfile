pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    tools {
        maven 'M2_home' 
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

                    withSonarQubeEnv('sam-sonarqube-server') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}
