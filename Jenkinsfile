pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from a GitHub repository
                    git branch: "${env.BRANCH_NAME}", credentialsId: '7466fc2c-8ba4-4282-9492-b13a7bd3a2ca', url: 'https://github.com/DevanshuTudip/pr-repo.git'
                }
            }
        }
        stage('test') {
            when {
              branch "PR-*"
            }
            steps {
                // Add your build steps here
                sh 'ls && date && cat index.html && sleep 5'
                sh 'printenv && env'
                echo "Pull request title: ${env.CHANGE_TITLE}"
                sh ' ${env.CHANGE_TITLE} | sed \'s/.*\(@.*\)/\1/\''
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
