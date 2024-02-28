pipeline {
    agent {
        label 'local'
    }

    environment {
        scannerHome = tool 'sonarqube'
        dependencyCheckHome = tool 'dp-check'
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

        stage('Dependency-Check') {
            steps {
                script {
                    // Run Dependency-Check scan
                    sh """
                        ${dependencyCheckHome}/bin/dependency-check.sh \
                            -s . \
                            -f JSON \
                            -o dependency-check-report.json
                    """
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
                echo 'Dependency-Check and SonarQube analysis completed successfully!'
            }
        }
    }

    post {
        always {
            echo 'Running the "test" job after the pipeline is completed.'
            build job: 'test', wait: false
        }
    }
}
