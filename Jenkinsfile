pipeline {
    agent any
  
    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Add your build steps here
            }
        }

        stage('Run Tests') {
            steps {
                // Add your test execution steps here
            }
        }

        stage('Copy HTML to Tomcat') {
            steps {
                node {
                    script {
                        def tomcatWebappsDir = "/var/lib/tomcat9/webapps"
                        def sourceHtmlPath

                        // Determine the source index.html based on the branch
                        if (env.BRANCH_NAME == 'Prod') {
                            sourceHtmlPath = 'index_prod.html'
                        } else if (env.BRANCH_NAME == 'Dev') {
                            sourceHtmlPath = 'index_dev.html'
                        } else {
                            error("Unsupported branch: ${env.BRANCH_NAME}")
                        }

                        // Copy the appropriate index.html to Tomcat in a separate context
                        def context = env.BRANCH_NAME.toLowerCase()
                        sh "mkdir -p ${tomcatWebappsDir}/${context}"
                        sh "cp ${sourceHtmlPath} ${tomcatWebappsDir}/${context}/index.html"
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                node {
                    // Assuming Tomcat is running and accessible
                    def context = env.BRANCH_NAME.toLowerCase()
                    sh "cp -r . ${TOMCAT_WEBAPPS}/${context}"
                }
            }
        }
    }
}
