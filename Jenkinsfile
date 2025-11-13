pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                python3 -m venv .venv
                . .venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . .venv/bin/activate
                export PYTHONPATH=$(pwd)
                pytest -q
                '''
            }
        }
    }

    post {
        always {
            script {
                // Get build status
                def status = currentBuild.currentResult

                // Send Webex message using bot token stored in Jenkins credentials
                withCredentials([string(credentialsId: 'WEBEX_BOT_TOKEN', variable: 'BOT_TOKEN')]) {
                    sh """
                    curl -s -X POST \
                      https://webexapis.com/v1/messages \
                      -H "Authorization: Bearer $BOT_TOKEN" \
                      -H "Content-Type: application/json" \
                      -d '{
                            "roomId": "Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vMjQzOTZiODAtYzA2OC0xMWYwLWJmYjgtYmQxNjQwYzc4M2Yz",
                            "text": "Jenkins build #${BUILD_NUMBER} - ${status} for pipeline ${JOB_NAME}"
                          }'
                    """
                }
            }
        }
    }
}