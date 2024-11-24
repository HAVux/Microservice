pipeline {
    agent any

    stages {
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonar1') { // Replace 'SonarQube' with the name of your configured server
                        sh """
                            /opt/sonar-scanner/bin/sonar-scanner \
                            -Dsonar.projectKey=recommendationservice \
                            -Dsonar.sources=. \
                            -Dsonar.java.binaries=. \
                            -Dsonar.host.url=$SONAR_HOST_URL
                        """
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                script {
                    sleep(5)
                    timeout(time: 1, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
        
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t vuhoang26/recommendationservice:latest ."
                    }
                }
            }
        }

        stage('Snyk Test') {
            steps {
                snykSecurity failOnError: false, failOnIssues: false, monitorProjectOnBuild: false, snykInstallation: 'snyk', snykTokenId: 'snyk-api-token'
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push vuhoang26/recommendationservice:latest "
                    }
                }
            }
        }
    }
}
