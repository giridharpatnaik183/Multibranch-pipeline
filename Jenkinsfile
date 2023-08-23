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

        stage('Build') {
            when {
                expression {
                    return env.BRANCH_NAME in ['dev', 'prod']
                }
            }
            steps {
                script {
                    // Your build steps here
                }
            }
        }

        stage('Test') {
            when {
                expression {
                    return env.BRANCH_NAME in ['dev', 'prod']
                }
            }
            steps {
                script {
                    // Your test steps here
                }
            }
        }

        stage('Deploy Dev to Tomcat') {
            when {
                expression {
                    return env.BRANCH_NAME == 'dev'
                }
            }
            steps {
                script {
                    def context = env.BRANCH_NAME.toLowerCase()

                    sh "mkdir -p ${TOMCAT_WEBAPPS}/${context}"
                    sh "cp index_dev.html ${TOMCAT_WEBAPPS}/${context}/index.html"
                }
            }
        }

        stage('Deploy Prod to Tomcat') {
            when {
                expression {
                    return env.BRANCH_NAME == 'prod'
                }
            }
            steps {
                script {
                    def context = env.BRANCH_NAME.toLowerCase()

                    sh "mkdir -p ${TOMCAT_WEBAPPS}/${context}"
                    sh "cp index_prod.html ${TOMCAT_WEBAPPS}/${context}/index.html"
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
