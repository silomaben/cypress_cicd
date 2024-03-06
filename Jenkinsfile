pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    stages {
        stage('Build Maven') {
            steps {
                git branch: 'dev',
                    credentialsId: '9e708a8d-c1d1-4a8a-9632-3b31ad932908',
                    url: 'https://github.com/silomaben/cypress_cicd.git'
            }
        }

        stage('Build and Package with Maven') {
            steps {
                // Use Docker image with Maven
                script {
                    // Run Maven commands inside a Docker container
                    withDockerContainer(image: 'maven:latest') {
                        sh 'mvn clean install'
                    }
                }
            }
        }

        stage('Build docker image') {
            steps {
                script {
                    bat """
                    docker build -t angular-docker .
                    docker run -p 4201:4200 angular-docker
                    """
                }
            }
        }
    }
}
