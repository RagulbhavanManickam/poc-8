pipeline {
    agent any

    stages {

        stage('Pull Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/RagulbhavanManickam/poc-8.git'
            }
        }

        stage('Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        def scannerHome = tool 'SonarQube-Scanner'
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=poc-8 \
                            -Dsonar.sources=."
                    }
                }
            }
        }

        stage('Deploy to Docker Server') {
            steps {
                sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.55 'docker pull nginx:latest && \
                    docker stop my-app || true && \
                    docker rm my-app || true && \
                    docker build -t my-app . && \
                    docker run -d --name my-app -p 80:80 my-app'
                '''
            }
        }
    }
}
