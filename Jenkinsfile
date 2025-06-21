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



        stage('Run API Tests') {
            steps {
                 bat "\"C:\\Users\\User\\AppData\\Roaming\\npm\\newman.cmd\" run \"%COLLECTION%\" --environment \"%ENVIRONMENT%\" --reporters cli,junit,htmlextra --reporter-junit-export \"%REPORT_XML%\" --reporter-htmlextra-export \"%REPORT_HTML%\""
            }
        }
    }

    post {
        always {
            echo 'Publishing reports even if tests failed.'

            // Publish JUnit XML
            junit allowEmptyResults: true, testResults: "${env.REPORT_XML}"

            // Publish HTML Report
            publishHTML(target: [
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: "${env.REPORT_HTML}",
                reportName: 'Newman HTML Report'
            ])
        }

        failure {
            echo "Build failed. Some API tests may have failed."
        }

        success {
            echo "Build and API tests completed successfully!"
        }
    }
}
