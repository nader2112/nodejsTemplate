pipeline {
    agent any
    environment {
        GIT_CREDENTIALS = credentials('id-github') // Remplacez 'id-github' par l'ID de vos identifiants GitHub configurés dans Jenkins
    }
    stages {
        stage('Clone repository') {
            steps {
                git branch: 'master', url: 'https://github.com/nader2112/nodejsTemplate.git', credentialsId: "${GIT_CREDENTIALS}"
            }
        }
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build Docker image') {
            steps {
                sh 'docker build -t my-node-app .'
            }
        }
        stage('Run Docker container') {
            steps {
                script {
                    def containerName = "my-node-app-container"
                    // Supprimez l'ancien conteneur s'il existe
                    sh "docker rm -f ${containerName} || true"
                    // Démarrez un nouveau conteneur sur un autre port
                    sh "docker run -d --name ${containerName} -p 3001:3000 my-node-app"
                }
            }
        }

        stage('Run Docker Compose') {
                    when {
                        expression {
                            fileExists('docker-compose-mongodb.yml') ||
                            fileExists('docker-compose-postgres.yml') ||
                            fileExists('docker-compose-mysql.yml')
                        }
                    }
                    steps {
                        script {
                            if (fileExists('docker-compose-mongodb.yml')) {
                                sh 'docker-compose -f docker-compose-mongodb.yml up -d'
                            } else if (fileExists('docker-compose-postgres.yml')) {
                                sh 'docker-compose -f docker-compose-postgres.yml up -d'
                            } else if (fileExists('docker-compose-mysql.yml')) {
                                sh 'docker-compose -f docker-compose-mysql.yml up -d'
                            }
                        }
                    }
                }

    }
}
