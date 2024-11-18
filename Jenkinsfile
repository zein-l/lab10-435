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
                    bat "venv\\Scripts\\activate.bat && coverage run -m unittest discover -s tests -p 'test_*.py'"
                }
            }
        }

        stage('Coverage') {
            steps {
                script {
                    bat "venv\\Scripts\\activate.bat && coverage report"
                    bat "venv\\Scripts\\activate.bat && coverage html"
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    bat "venv\\Scripts\\activate.bat && bandit -r ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
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
