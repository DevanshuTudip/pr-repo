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
        stage('fetch tag and env from the PR') {
            when {
                branch "PR-*"
            }
            steps {
                // Add your build steps here
                sh 'ls && date && cat index.html' 
                sh 'printenv && env'
                echo "Pull request title: ${env.CHANGE_TITLE}"
                script {
                    def prNumber = env.CHANGE_ID // Extracting the PR number from the CHANGE_ID
                    def apiUrl = "https://api.github.com/repos/DevanshuTudip/pr-repo/pulls/${env.CHANGE_ID}"
                    def response = httpRequest(url: apiUrl, authentication: '02296c52-d341-452c-9740-94f2bda25732')
                    def labels = response.getData().labels.collect { it.name }
                    echo "response: ${response}"
                    echo "PR Labels: ${labels}"

                    env.env = sh(
                        script: "echo \${CHANGE_TITLE} | sed -n 's/.*\\(@[[:alnum:]]*\\).*/\\1/p'",
                        returnStdout: true
                    ).trim()
                    env.tag = sh(
                        script: "echo \${CHANGE_TITLE} | sed -n 's/.*#\\([[:alnum:]]*\\)\\( @\\)*.*/\\1/p'",
                        returnStdout: true
                    ).trim()
                    // if (tag == '' || env == '') {
                    //     currentBuild.result = 'ABORTED'
                    //     error('Tag or environment not found in the PR title. Aborting the pipeline.')
                    // }
                }
            }
        }
        stage('Using the Env variable in another step') {
            when {
                branch "PR-*"
            }
            steps {
               echo "npx nx run cucumber-tests:test:qa --tag=${env.tag} --env=${env.env}"
            }
        }
    }


    // Add post actions if needed
    // post {
    //     success {
    //         echo 'Pipeline executed successfully'
    //     }
    //     failure {
    //         echo 'Pipeline failed'
    //     }
    // }
}
