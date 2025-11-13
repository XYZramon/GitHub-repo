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
                # Create virtual environment
                python3 -m venv .venv

                # Activate venv and install dependencies
                . .venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                # Activate venv and run pytest
                . .venv/bin/activate
                pytest -q
                '''
            }
        }
    }
}