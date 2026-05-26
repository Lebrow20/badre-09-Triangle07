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
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {

                    bat '''
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                        docker build -t fanilonyavo/triang7:1.0.0 .
                        docker push fanilonyavo/triang7:1.0.0
                    '''
                }
            }
        }
    }

    post {
        failure {
            emailext(
                subject: "Build FAILED - ${JOB_NAME}",
                body: "Ce Build ${BUILD_NUMBER} a échoué",
                to: "badre.bousalem@enpc.fr"
            )
        }
    }
}