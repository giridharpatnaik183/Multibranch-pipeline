pipeline {
    agent any

    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps' // Set this to the actual path of the Tomcat webapps directory
    }

    stages {
        stage('Print Debug Info') {
            steps {
                script {
                    echo "Branch Name: ${env.BRANCH_NAME}"
                    echo "Deploy Dev on Tomcat: ${env.BRANCH_NAME == 'dev'}"
                    echo "Deploy Prod on Tomcat: ${env.BRANCH_NAME == 'prod'}"
                }
            }
        }

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
                    def deployDev = env.BRANCH_NAME == 'dev'
                    echo "Deploy Dev on Tomcat: ${deployDev}"
                    return deployDev
                }
            }
            steps {
                script {
                    def tomcatWebappsDir = "/var/lib/tomcat9/webapps"
                    def sourceHtmlPath = 'index_dev.html'

                    def context = env.BRANCH_NAME.toLowerCase()

                    sh "mkdir -p ${tomcatWebappsDir}/${context}"
                    sh "cp ${sourceHtmlPath} ${tomcatWebappsDir}/${context}/index.html"
                }
            }
        }

        stage('Deploy Prod on Tomcat') {
            when {
                expression {
                    def deployProd = env.BRANCH_NAME == 'prod'
                    echo "Deploy Prod on Tomcat: ${deployProd}"
                    return deployProd
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
