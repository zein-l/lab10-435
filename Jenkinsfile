pipeline {
    agent any

    environment {
        VIRTUAL_ENV = 'venv'
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}/${VIRTUAL_ENV}")) {
                        sh "python -m venv ${VIRTUAL_ENV}"
                    }
                    sh ". ${VIRTUAL_ENV}/bin/activate && pip install -r requirements.txt"
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    sh ". ${VIRTUAL_ENV}/bin/activate && flake8 app.py"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh ". ${VIRTUAL_ENV}/bin/activate && coverage run -m unittest discover"
                }
            }
        }

        stage('Coverage') {
            steps {
                script {
                    sh ". ${VIRTUAL_ENV}/bin/activate && coverage report"
                    sh ". ${VIRTUAL_ENV}/bin/activate && coverage html"
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
                    sh ". ${VIRTUAL_ENV}/bin/activate && bandit -r ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    sh ". ${VIRTUAL_ENV}/bin/activate && python app.py"
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
