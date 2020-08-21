properties([
        parameters(
                [
                        stringParam(
                                name: 'GIT_REPO',
                                defaultValue: ''
                        ),
                        stringParam(
                                name: 'VERSION',
                                defaultValue: ''
                        ),
                        choiceParam(
                                name: 'ENV',
                                choices: ['test', 'staging', 'production']
                        )
                ]
        )
])

pipeline {

    agent {
        kubernetes {
            label 'deploy-service-pod'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    job: deploy-service
spec:
  containers:
  - name: helm
    image: alpine/helm:3.2.4
    command: ["cat"]
    tty: true
"""
        }
    }

    stages {

        stage('Deploy to env') {
            steps {
                container('helm') {
                    script {
                        sh "env"
                        sh "helm repo add stable https://kubernetes-charts.storage.googleapis.com"
                        sh "helm upgrade --install mysql stable/mysql --namespace ${ENV}" 
                    }
                }
            }
        }
    }
}

