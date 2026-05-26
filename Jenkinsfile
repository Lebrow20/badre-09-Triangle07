pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Git Checkout') {
            steps {
                git credentialsId: 'git_credentials',
                    url: 'https://github.com/Lebrow20/badre-09-Triangle07.git'
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
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat '''
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                        docker build -t %DOCKER_USER%/triang7:1.0.0 .
                        docker push %DOCKER_USER%/triang7:1.0.0
                    '''
                }
            }
        }
    }

    post {
        failure {
            emailext(
                subject: "Jenkins Build Failed",
                body: "Le build ${BUILD_NUMBER} a échoué.",
                to: "nyavofanilo.rabe@gmail.com"
            )
        }

        success {
            emailext(
                subject: "Jenkins Build Success",
                body: "Le build ${BUILD_NUMBER} a réussi.",
                to: "nyavofanilo.rabe@gmail.com"
            )
        }
    }
}