pipeline {
    agent any

    environment {
        VIRTUAL_ENV = 'venv'
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt"
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && flake8 app.py"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && coverage run -m unittest discover"
                }
            }
        }

        stage('Coverage') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && coverage report"
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && coverage html"
                }
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'htmlcov',
                    reportFiles: 'index.html',
                    reportName: 'Coverage Report'
                ])
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && bandit -r ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    bat "${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate && python app.py"
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
