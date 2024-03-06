
pipeline {
    agent {
        node {
            
            label 'alpha'
        }
    }

    parameters {
        choice(name: 'BROWSER', choices:['chrome','edge'], description: "Choose browser to run scripts")
    }

    tools{nodejs 'node20.11'}

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
                
                sh 'TMPDIR=/home/jenkins/workspace/Cypress_Deploy_dev/src npm install'
            }
        }
        


        
              stage('Run UI') {
                    steps {
                       script {
                            sh"cd /home/jenkins/workspace/Cypress_Deploy_dev/ && ls -al"
                            sh"npm install -g @angular/cli"
                            
                            sh(script: 'nohup ng serve &', returnStatus: true)
                           
                        }
                    }
                }

                stage('Run Cypress Tests') {
                    steps {
                        wrap([$class: 'Xvfb']) {
                            
                            // Run Cypress Tests
                            sh "npx cypress open" // Use 'bat' for Windows command
                        }
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

    post {
        always {
            archiveArtifacts artifacts: 'cypress/reports/**', allowEmptyArchive: true
        }
    }
}
