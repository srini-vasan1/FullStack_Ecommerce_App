pipeline {
    agent { 
        label 'local'
    }
    
    environment {
        scannerHome = tool 'sonarqube'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from the Git repository
                    checkout scm
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=ecommerce \
                                -Dsonar.sources=. \
                                -Dsonar.host.url=http://192.168.0.100:9000
                        """
                    }
                }
            }
        }

        stage('Post-Analysis') {
            steps {
                echo 'SonarQube analysis completed successfully!'
            }
        }
    }
}
