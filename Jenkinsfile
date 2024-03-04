
pipeline {
    agent any

    parameters {
        choice(name: 'BROWSER', choices:['chrome','edge'], description: "Choose browser to run scripts")
    }

    // options {
    //     buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '10'))
    // }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: '9e708a8d-c1d1-4a8a-9632-3b31ad932908',
                    url: 'https://github.com/silomaben/cypress_cicd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install' 
            }
        }

        stage('Run UI and Cypress Tests in Parallel') {
            parallel {
                stage('Run UI') {
                    steps {
                       script {
                            // run the UI
                            bat(script: 'start /B ng serve', returnStatus: true)
                           
                        }
                    }
                }
                stage('Run Cypress Tests') {
                    steps {
                            // Run Cypress Tests
                            bat 'npx cypress run --browser chrome' // Use 'bat' for Windows command
                        
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'cypress/reports/**', allowEmptyArchive: true
        }
    }
}
