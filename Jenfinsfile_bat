
pipeline {
    agent any

    parameters {
        choice(name: 'BROWSER', choices:['chrome','edge'], description: "Choose browser to run scripts")
    }

    // tools{nodejs 'node20.11'}

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'dev',
                    credentialsId: '9e708a8d-c1d1-4a8a-9632-3b31ad932908',
                    url: 'https://github.com/silomaben/cypress_cicd.git'
            }
        }


        stage('Install Dependencies') {
            steps {
                bat "npm install" 
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
                            bat "npx cypress run --browser ${params.BROWSER}" // Use 'bat' for Windows command
                        
                    }
                }


                stage('Deploy') {
                    steps {
                        script {
                            // Determine the branch name
                            def branchName = env.BRANCH_NAME
                            echo "Branch Name: ${branchName}"


                            // Decide which environment to deploy based on the branch name
                            if (branchName == 'main') {
                                echo "deploying to production"
                            } else if(branchName == 'dev') {
                                echo "deploying to development now"
                            } else if(branchName == 'QA') {
                                echo "deploying to QA now"

                            }
                        }
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
