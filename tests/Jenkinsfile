pipeline {
    agent any

    environment {
        PYTHON_HOME = 'C:\\Users\\Zeink\\AppData\\Local\\Programs\\Python\\Python312' // Adjust if your Python path is different
        PATH = "${env.PYTHON_HOME};${env.PYTHON_HOME}\\Scripts;${env.PATH}"
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    bat "${env.PYTHON_HOME}\\python -m venv venv"
                    bat "venv\\Scripts\\activate.bat && ${env.PYTHON_HOME}\\Scripts\\pip install -r requirements.txt"
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
                    bat "venv\\Scripts\\activate.bat && coverage run -m unittest discover"
                }
            }
        }

        stage('Coverage') {
            steps {
                script {
                    bat "venv\\Scripts\\activate.bat && coverage report"
                    bat "venv\\Scripts\\activate.bat && coverage html"
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
                    bat "venv\\Scripts\\activate.bat && bandit -r ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    bat "venv\\Scripts\\activate.bat && python app.py"
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
