pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv .venv
                    .venv/bin/python -m pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '.venv/bin/python -m pytest -v'
            }
        }

        stage('Run Application') {
            steps {
                sh '.venv/bin/python app.py'
            }
        }

        stage('Package Release') {
            steps {
                sh '''
                    mkdir -p release
                    cp app.py requirements.txt release/
                '''
                archiveArtifacts artifacts: 'release/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: All stages completed.'
        }
        failure {
            echo 'FAILED: Check Console Output.'
        }
    }
}
