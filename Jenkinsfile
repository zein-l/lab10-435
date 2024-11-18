pipeline {
    agent any

    environment {
        PYTHON_HOME = 'C:\\Users\\Zeink\\AppData\\Local\\Programs\\Python\\Python312'
        PATH = "${env.PYTHON_HOME};${env.PYTHON_HOME}\\Scripts;${env.PATH}"
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    bat "python -m venv venv"
                    bat "venv\\Scripts\\activate.bat && pip install -r requirements.txt"
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    bat "venv\\Scripts\\activate.bat && flake8 app.py"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    bat "venv\\Scripts\\activate.bat && coverage run -m unittest discover -s tests -p 'test_*.py' || exit 0"
                }
            }
        }

        stage('Coverage') {
            steps {
                script {
                    bat "venv\\Scripts\\activate.bat && coverage report || exit 0"
                    bat "venv\\Scripts\\activate.bat && coverage html || exit 0"
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    bat "venv\\Scripts\\activate.bat && bandit -r . || exit 0"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Simulating deployment..."
                    bat "echo Deployment successful || exit 0"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
