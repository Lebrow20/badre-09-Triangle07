pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/Lebrow20/badre-09-Triangle07.git'
            }
        }

        stage('Build Application') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Docker Build & Push') {
            steps {

                withCredentials([
                    string(credentialsId: 'dockerhub', variable: 'DOCKER_PASS')
                ]) {

                    bat '''
                        echo %DOCKER_PASS% | docker login -u fanilonyavo --password-stdin
                        docker build -t fanilonyavo/triang7:1.0.0 .
                        docker push fanilonyavo/triang7:1.0.0
                    '''
                }
            }
        }
    }

    post {
        always {
            emailext(
                subject: "Jenkins Build ${BUILD_NUMBER}",
                body: "Le build est terminé avec le statut : ${currentBuild.currentResult}",
                to: "nyavofanilo.rabe@gmail.com"
            )
        }
    }
}