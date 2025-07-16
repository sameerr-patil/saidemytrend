pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
        scannerHome = tool 'sam-sonarqube-scanner'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sam-sonarqube--server') {
                    sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=sam01-key-saidemytrend \
                        -Dsonar.organization=sam01-key \
                        -Dsonar.sources=src \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    // Wait for quality gate result, but do not fail the build even if gate fails
                    waitForQualityGate abortPipeline: false
                }
            }
        }

        stage('Publish JAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                echo 'JAR published to Jenkins workspace archive.'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed (regardless of result).'
        }
    }
}

