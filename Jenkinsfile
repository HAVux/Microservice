pipeline {
    agent any
    
    stages {

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonar1') { // Replace 'SonarQube' with the name of your configured server
                        sh """
                            /opt/sonar-scanner/bin/sonar-scanner \
                            -Dsonar.projectKey=frontend \
                            -Dsonar.sources=. \
                            -Dsonar.java.binaries=build \
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
        
        // stage('Build & Tag Docker Image') {
        //     steps {
        //         script {
        //             withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
        //                 sh "docker build -t vuhoang26/frontend:latest ."
        //             }
        //         }
        //     }
        // }

        stage('Snyk Test') {
            steps {
                snykSecurity failOnError: false, failOnIssues: false, monitorProjectOnBuild: false, snykInstallation: 'snyk', snykTokenId: 'snyk-api-token'
            }
        }
        
    //     stage('Push Docker Image') {
    //         steps {
    //             script {
    //                 withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
    //                     sh "docker push vuhoang26/frontend:latest"
    //                 }
    //             }
    //         }
    //     }
    }

    post {
    // Clean after build
    always {
        cleanWs(cleanWhenNotBuilt: false,
                deleteDirs: true,
                disableDeferredWipeout: true,
                notFailBuild: true,
                patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                        [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }
}
