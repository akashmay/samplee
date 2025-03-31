pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Clone the repository
                    checkout([$class: 'GitSCM',
                        branches: [[name: '*/main']], // Change branch if needed
                        userRemoteConfigs: [[url: 'https://github.com/akashmay/samplee.git']]
                    ])
                }
            }
        }

        stage('Run Python Tests') {
            steps {
                script {
                    // Run the Python test container, execute tests, and remove the container after execution
                    sh '''
                        docker run --rm --name python-tests-container \
                        -v ${WORKSPACE}:/app \
                        -w /app \
                        python:3.9 bash -c "
                            python -m venv venv &&
                            source venv/bin/activate &&
                            pip install --upgrade pip &&
                            pip install -r requirements.txt &&
                            pytest tests --html=reports/report.html
                        "
                    '''
                }
            }
        }

        stage('Publish HTML Report') {
            steps {
                publishHTML (target: [
                    reportDir: 'reports',
                    reportFiles: 'report.html',
                    reportName: 'Test Report'
                ])
            }
        }
    }
}
