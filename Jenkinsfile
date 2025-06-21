pipeline {
    agent any

    environment {
        COLLECTION = 'ReqRes Collection-API Testing and Debugging-Monitoring.postman_collection.json'
        ENVIRONMENT = 'SystemTest.postman_environment.json'
        REPORT_XML = 'newman-report.xml'
        REPORT_HTML = 'newman-report.html'
    }


    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/RadhaReddyG/ReqRes-API-Tests.git', branch: 'main'
            }
        }

        stage('Install Newman & Reporter') {
            steps {
                sh 'npm install -g newman newman-reporter-htmlextra'
            }
        }

        stage('Run API Tests') {
            steps {
                sh """
                newman run $COLLECTION \
                    --environment $ENVIRONMENT \
                    --reporters cli,junit,htmlextra \
                    --reporter-junit-export $REPORT_XML \
                    --reporter-htmlextra-export $REPORT_HTML
                """
            }
        }

        stage('Publish JUnit Results') {
            steps {
                junit "$REPORT_XML"
            }
        }

        stage('Publish HTML Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '.',
                    reportFiles: "$REPORT_HTML",
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
