pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'echo Creating virtual environment and installing dependencies...'
            }
        }
        stage('Test') {
            steps {
                bat 'echo Running tests...'
                bat 'python -m unittest discover -s .'
            }
        }
        stage('Deploy') {
            steps {
                bat 'echo Deploying application...'
                bat "mkdir %WORKSPACE%\\python-app-deploy"
                bat "copy %WORKSPACE%\\app.py %WORKSPACE%\\python-app-deploy\\"
            }
        }
        stage('Run Application') {
            steps {
                bat 'echo Running application...'
                bat "start /B python %WORKSPACE%\\python-app-deploy\\app.py > %WORKSPACE%\\python-app-deploy\\app.log 2>&1"
                bat "echo %PROCESS_ID% > %WORKSPACE%\\python-app-deploy\\app.pid"
            }
        }
        stage('Test Application') {
            steps {
                bat 'echo Testing application...'
                bat "python %WORKSPACE%\\test_app.py"
            }
        }
    }

    post {
        success {
            bat 'echo Pipeline completed successfully!'
        }
        failure {
            bat 'echo Pipeline failed. Check the logs for more details.'
        }
    }
}
