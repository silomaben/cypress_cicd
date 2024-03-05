pipeline {
    agent {
        node {
            label 'alpha'
        }
    }

    parameters {
        choice(name: 'BROWSER', choices:['chrome','edge'], description: "Choose browser to run scripts")
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout code using 'sh' for cross-platform compatibility
                    sh 'git branch: main --credentials 9e708a8d-c1d1-4a8a-9632-3b31ad932908 https://github.com/silomaben/cypress_cicd.git'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                // Use 'sh' for running shell commands
                sh 'npm install'
            }
        }

        stage('Run UI and Cypress Tests in Parallel') {
            parallel {
                stage('Run UI') {
                    steps {
                        script {
                            // run the UI
                            sh 'start /B ng serve' // You may need to adjust this based on your specific command
                        }
                    }
                }
                stage('Run Cypress Tests') {
                    steps {
                        // Run Cypress Tests using 'sh'
                        sh "npx cypress run --browser ${params.BROWSER}"
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
