pipeline {
    agent any

    stages {
        stage('Build Node.js Container') {
            steps {
                script {
                    docker.build('zubair/node:14.1', '-f Dockerfile.node .').inside {
                        sh 'node -v'
                        sh 'echo Node $(node -v) - Zubair'
                    }
                }
            }
        }
        stage('Build Maven Container') {
            steps {
                script {
                    docker.build('zubair/maven:3.8.1', '-f Dockerfile.maven .').inside {
                        sh 'mvn -v'
                        sh 'echo Maven $(mvn -v) - Zubair'
                    }
                }
            }
        }
    }
}
