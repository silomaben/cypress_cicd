// pipeline{
//     agent any

//     parameters{
//         string(name: 'SPEC', defaultValue: "cypress/intergration/**/**", description: "Enter the scripts that you want to execute")
//         choice(name: 'BROWSER', choices:['chrome','edge'], description: "Choose browser to run scripts")
//     }

//     // options{
//     //     ansiColor('xterm')
//     // }

//     options {
//     buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '10'))
// }


//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'main',
//                     credentialsId: '9e708a8d-c1d1-4a8a-9632-3b31ad932908',
//                     url: 'https://github.com/silomaben/cypress_cicd.git'
//             }
//         }

//         // stage('Connect to VPN') {
//         //     steps {
//         //         script {
//         //             // Replace 'vpn-server', 'username', and 'password' with your actual VPN server details
//         //             sh 'sudo openconnect --user=benard.masikonde --password=Sikumbuki01@! benard.masikonde@aws-vpn.cerebriai.com'
//         //         }
//         //     }
//         // }

//         stage('Install Dependencies') {
//             steps {
//                 bat 'npm install' // Use 'bat' for Windows command
//             }
//         }
        
//         stage('Run UI') {
//             steps {
//                 bat 'ng serve' // Use 'bat' for Windows command
//             }
//         }

//         stage('Run Cypress Tests') {
//             steps {
//                 bat 'npx cypress run --browser chrome' // Use 'bat' for Windows command
//             }
//         }
//     }

//     post{
//         always{
//             archiveArtifacts artifacts: 'cypress/reports/**', allowEmptyArchive: true

//         }
//     }
// }

pipeline {
    agent any

    parameters {
        string(name: 'SPEC', defaultValue: "cypress/intergration/**/**", description: "Enter the scripts that you want to execute")
        choice(name: 'BROWSER', choices:['chrome','edge'], description: "Choose browser to run scripts")
    }

    options {
        buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '10'))
    }

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
                bat 'npm install' // Use 'bat' for Windows command
            }
        }

        stage('Run UI and Cypress Tests in Parallel') {
            parallel {
                stage('Run UI') {
                    steps {
                       script {
                            // Run UI and store the process ID
                            def uiProcess = bat(script: 'start /B ng serve', returnStatus: true)
                            // echo "UI Process ID: $uiProcess"
                            // timeout(time: 15, unit: 'MINUTES') {
                            //     // Wait for a reasonable time
                            //     // Your other UI-related steps here
                            // }

                            // // After the timeout, kill the UI process
                            // echo "Killing UI process after timeout"
                            // bat "taskkill /F /PID $uiProcess"
                        }
                    }
                }
                stage('Run Cypress Tests') {
                    steps {
                        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                            // Run Cypress Tests
                            bat 'npx cypress run --browser chrome' // Use 'bat' for Windows command
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                // Check if the report file exists
                if (fileExists('C:\\Users\\kben\\AppData\\Local\\Jenkins\\.jenkins\\workspace\\JituTest\\cypress\\reports\\html\\index.html')) {
                    echo "Report file found. Killing the UI process."
                    // Kill the UI process
                    bat 'taskkill /F /PID $uiProcess'
                } else {
                    echo "Report file not found. UI process continues."
                }
            }
            archiveArtifacts artifacts: 'cypress/reports/**', allowEmptyArchive: true
        }
    }
}
