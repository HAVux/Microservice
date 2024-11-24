pipeline {
    agent any

    stages {
        stage('Deploy To Kubernetes') {
            steps {
                sh "kubectl apply -f deployment-service.yml -n webapps"
            }
        }
        
        stage('verify Deployment') {
            steps {
                sh "kubectl get svc -n webapps"
            }
        }
    }
}
