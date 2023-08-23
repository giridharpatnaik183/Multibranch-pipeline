pipeline {
    agent any

    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps' // Set this to the actual path of the Tomcat webapps directory
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                checkout scm
            }
        }

        stage('Build and Test') {
            when {
                expression {
                    return env.BRANCH_NAME in ['dev', 'prod']
                }
            }
            steps {
                script {
                    // Your build and test steps here
                    // For example:
                    sh 'npm install'
                    sh 'npm run test'
                }
            }
        }

        stage('Deploy to Tomcat') {
            when {
                expression {
                    return env.BRANCH_NAME in ['dev', 'prod']
                }
            }
            steps {
                script {
                    def context = env.BRANCH_NAME.toLowerCase()

                    sh "mkdir -p ${TOMCAT_WEBAPPS}/${context}"
                    sh "cp index_${context}.html ${TOMCAT_WEBAPPS}/${context}/index.html"
                }
            }
        }
    }
    
    post {
        always {
            script {
                currentBuild.result = currentBuild.resultIsBetterAs(Result.SUCCESS) ? Result.SUCCESS : currentBuild.result
            }
        }
    }
}
