pipeline {
    agent any

    environment {
        IMAGE_NAME = 'dorasanamma/jmetertesting:v1.0'  // Your Docker image name
        TEST_REPO = 'https://github.com/Dorasani2000/JMeter-Scripts.git'  // Your GitHub repo URL
    }

    stages {
        stage('Checkout Tests') {
            steps {
                // Clone your test scripts repository with credentials
                git branch: 'main', credentialsId: "${params.gitCredentialsID}", url: "${TEST_REPO}"
            }
        }
        
        stage('Run JMeter Tests') {
            steps {
                // Use bat instead of sh and adjust volume mount for Windows PowerShell syntax
                bat """
                docker run --rm -v %cd%\\:/tests -w /tests ${IMAGE_NAME} ^
                -n -t Account_Summary_Balance.jmx -l results.jtl
                """
            }
        }

        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: 'results.jtl, report/**', allowEmptyArchive: true
            }
        }
    }
}
