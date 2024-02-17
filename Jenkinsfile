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
                sh 'npm install htmlhint-cli'
            }
        }
        stage('Test') {
            steps {
                script {
                    def testExitCode = sh(script: 'npx htmlhint /*.html', returnStatus: true)
                    if (testExitCode == 0) {
                        echo 'Tests passed, proceeding with deployment'
                        currentBuild.result = 'SUCCESS'
                    } else {
                        echo 'Tests failed, deployment aborted'
                        currentBuild.result = 'FAILURE'
                        error 'Tests failed, deployment aborted'
                    }
                }
            }
        }
        stage('Deployment') {
            when {
                expression { currentBuild.result == 'SUCCESS' }
            }
            steps {
                sh 'sudo -S sh -c "cat ./test.html > /var/www/html/index.nginx-debian.html"'
                sh 'sudo systemctl restart nginx'
            }
        }
    }
}
