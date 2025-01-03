pipeline {
    agent any

    environment {
        DEPLOY_DIR = "${WORKSPACE}/python-app-deploy"
    }

    stages {
        stage('Setup') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                sh '''
                python3 -m venv venv
                source venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                source venv/bin/activate
                python3 -m unittest discover -s .
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh '''
                mkdir -p ${DEPLOY_DIR}
                cp app.py ${DEPLOY_DIR}/
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo 'Running application...'
                sh '''
                nohup python3 ${DEPLOY_DIR}/app.py > ${DEPLOY_DIR}/app.log 2>&1 &
                echo $! > ${DEPLOY_DIR}/app.pid
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo 'Testing application...'
                sh '''
                source venv/bin/activate
                python3 test_app.py
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}
