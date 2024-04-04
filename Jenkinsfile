pipeline {
    agent any
    
    stages {
  
        stage('Test') {
            steps {
                script {
                    // Run Python Selenium script
                    def output = sh(script: 'python3 test_script.py', returnStdout: true).trim()
                    println "Output of test_script.py: ${output}"
                    // Check the output to determine the test condition
                    if (output.contains('Passed')) {
                        currentBuild.result = 'SUCCESS'
                    } else {
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
        stage('Deployment') {
            when {
                expression {
                    // Only run the Deployment stage if the Test stage was successful
                    return currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    // Enclose shell steps in curly braces
                    sh 'sudo -S cp /var/lib/jenkins/workspace/newP/*.html /var/www/html/'
                    sh 'sudo -S systemctl restart nginx'
                }
            }
        }
    }
}
