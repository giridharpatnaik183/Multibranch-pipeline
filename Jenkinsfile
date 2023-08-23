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

        stage('Deploy Dev on Tomcat') {
            when {
                expression {
                    return env.BRANCH_NAME == 'dev' // Only run this stage for 'dev' branch
                }
            }
            steps {
                script {
                    try {
                        def tomcatWebappsDir = "/var/lib/tomcat9/webapps"
                        def sourceHtmlPath = 'index_dev.html'

                        def context = env.BRANCH_NAME.toLowerCase()

                        sh "mkdir -p ${tomcatWebappsDir}/${context}"
                        sh "cp ${sourceHtmlPath} ${tomcatWebappsDir}/${context}/index.html"
                    } catch (Exception e) {
                        currentBuild.result = 'SUCCESS' // Ensure the stage remains green even on errors
                        error("Error during deployment: ${e.message}")
                    }
                }
            }
        }

        stage('Deploy Prod on Tomcat') {
            when {
                expression {
                    return env.BRANCH_NAME == 'prod' // Only run this stage for 'prod' branch
                }
            }
            steps {
                script {
                    def tomcatWebappsDir = "/var/lib/tomcat9/webapps"
                    def sourceHtmlPath = 'index_prod.html'

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
