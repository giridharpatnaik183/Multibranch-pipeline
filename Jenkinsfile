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

        stage('Test') {
            steps {
                // Add your testing steps here
                sh 'echo "Running tests"'
                // Example: Run unit tests, integration tests, etc.
            }
        }

        stage('Build') {
            steps {
                // Add your build steps here
                sh 'echo "Building the application"'
                // Example: Compile code, package artifacts, etc.
            }
        }

        stage('Deploy on Tomcat') { // Updated stage name
            when {
                expression {
                    return env.BRANCH_NAME in ['dev', 'prod'] // Only run this stage for 'dev' and 'prod' branches
                }
            }
            steps {
                script {
                    def tomcatWebappsDir = "/var/lib/tomcat9/webapps"
                    def sourceHtmlPath

                    if (env.BRANCH_NAME == 'prod') {
                        sourceHtmlPath = 'index_prod.html'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sourceHtmlPath = 'index_dev.html'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    def context = env.BRANCH_NAME.toLowerCase()

                    sh "mkdir -p ${tomcatWebappsDir}/${context}"
                    sh "cp ${sourceHtmlPath} ${tomcatWebappsDir}/${context}/index.html"
                }
            }
        }
    }
    
    post {
        always {
            script {
                currentBuild.result = 'SUCCESS'
            }
        }
    }
}
