pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from a GitHub repository
                    git branch: "${env.BRANCH_NAME}", credentialsId: 'your-credentials-id', url: 'https://github.com/your-username/your-repo.git'
                }
            }
        }
        stage('test') {
            steps {
                // Add your build steps here
                sh 'ls && date'
            }
        }
    }

    // Add post actions if needed
    post {
        success {
            echo 'Pipeline executed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}

