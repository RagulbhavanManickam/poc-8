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
                sh """
                    scp -o StrictHostKeyChecking=no Dockerfile ubuntu@172.31.30.55:/home/ubuntu/
                    scp -o StrictHostKeyChecking=no index.html ubuntu@172.31.30.55:/home/ubuntu/
                    ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.55 '
                        sudo docker stop my-app || true
                        sudo docker rm my-app || true
                        sudo docker build -t my-app /home/ubuntu/
                        sudo docker run -d --name my-app -p 80:80 my-app
                    '
                """
            }
        }
    }
}
