pipeline {
    agent any

    environment {
        COLLECTION = 'ReqRes Collection-API Testing and Debugging-Monitoring.postman_collection.json'
        ENVIRONMENT = 'SystemTest.postman_environment.json'
        REPORT_XML = 'newman-report.xml'
        REPORT_HTML = 'newman-report.html'
        PATH = "C:\\Users\\User\\AppData\\Roaming\\npm;${env.PATH}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/RadhaReddyG/ReqRes-API-Tests.git', branch: 'main'
            }
        }

        stage('Install Newman & Reporter') {
            steps {
                bat 'npm install -g newman newman-reporter-htmlextra'
            }
        }

        stage('Run API Tests') {
            steps {
                bat "\"C:\\Users\\User\\AppData\\Roaming\\npm\\newman.cmd\" run \"%COLLECTION%\" --environment \"%ENVIRONMENT%\" --reporters cli,junit,htmlextra --reporter-junit-export \"%REPORT_XML%\" --reporter-htmlextra-export \"%REPORT_HTML%\""
            }
        }

        stage('Publish JUnit Results') {
            steps {
                junit "${env.REPORT_XML}"
            }
        }

        stage('Publish HTML Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '.',
                    reportFiles: "${env.REPORT_HTML}",
                    reportName: 'Newman API Report'
                ])
            }
        }
    }

    post {
        failure {
            echo "Build failed. API tests may have failed."
        }
        success {
            echo "Build and API tests completed successfully!"
        }
    }
}
