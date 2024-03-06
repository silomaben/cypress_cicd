

pipeline {
    agent any
    tools{
        maven 'maven3'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Java-Techie-jt/devops-automation']]])
                bat 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    bat """
                    
                    docker build -t angular-docker .


                    docker run -p 4201:4200 angular-docker

                    """
                }
            }
        }
        
    }
}
