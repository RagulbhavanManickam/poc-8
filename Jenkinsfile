pipeline {
    agent any

    stages {

        stage('Pull Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-username/poc-8.git'
            }
        }

        stage('Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        sonar-scanner \
                          -Dsonar.projectKey=poc-8 \
                          -Dsonar.sources=.
                    '''
                }
            }
        }

        stage('Deploy to Docker Server') {
            steps {
                sh '''
                    ssh ubuntu@<your-docker-server-private-ip> "
                        docker pull nginx:latest
                        docker stop my-app || true
                        docker rm my-app || true
                        docker build -t my-app .
                        docker run -d --name my-app -p 80:80 my-app
                    "
                '''
            }
        }
    }
}
