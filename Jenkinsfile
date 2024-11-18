pipeline {
    agent any

    environment {
        PYTHON_HOME = 'C:\\Users\\Zeink\\AppData\\Local\\Programs\\Python\\Python312' // Adjust if your Python is in a different location
        PATH = "${env.PYTHON_HOME};${env.PYTHON_HOME}\\Scripts;${env.PATH}"
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    bat "${env.PYTHON_HOME}\\python -m venv venv"
                    bat "${env.PYTHON_HOME}\\Scripts\\activate && pip install -r requirements.txt"
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    bat "${env.PYTHON_HOME}\\Scripts\\activate && flake8 app.py"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    bat "${env.PYTHON_HOME}\\Scripts\\activate && coverage run -m unittest discover"
                }
            }
        }

        stage('Coverage') {
            steps {
                script {
                    bat "${env.PYTHON_HOME}\\Scripts\\activate && coverage report"
                    bat "${env.PYTHON_HOME}\\Scripts\\activate && coverage html"
                }
                publishHTML(target: [
                    reportDir: 'htmlcov',
                    reportFiles: 'index.html',
                    reportName: 'Coverage Report'
                ])
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    bat "${env.PYTHON_HOME}\\Scripts\\activate && bandit -r ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    bat "${env.PYTHON_HOME}\\Scripts\\activate && python app.py"
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
