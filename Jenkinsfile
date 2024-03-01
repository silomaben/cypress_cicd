pipeline{
    agent any

    parameters{
        string(name: 'SPEC', defaultValue: "cypress/intergration/**/**", description: "Enter the scripts that you want to execute")
        choice(name: 'BROWSER', choices:['chrome','edge'], description: "Choose browser to run scripts")
    }

    // options{
    //     ansiColor('xterm')
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
                bat 'npm install' // Use 'bat' for Windows command
            }
        }
        
        // stage('Run UI') {
        //     steps {
        //         bat 'ng serve --no-interactive' // Use 'bat' for Windows command
        //     }
        // }

        stage('Run Cypress Tests') {
            steps {
                bat 'npx cypress run --browser ${BROWSER} --spec ${SPEC}' // Use 'bat' for Windows command
            }
        }
       
    }

    post{
        always{
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'cypress/reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
        }
}