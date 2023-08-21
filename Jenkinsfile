pipeline {
    agent any
    
    tools {
        maven 'maven'
    }
    
    stages {
        stage('SCM Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master'], [name: '*/qa']], userRemoteConfigs: [[url: 'https://github.com/giridharpatnaik183/webAppExample.git']]])
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat Server') {
            steps {
                deploy adapters: [tomcat9(credentialsId: '058ddaec-e159-4481-b9e6-801f583943b6', path: '', url: 'http://18.234.157.123:8090/')], contextPath: null, war: '**/*.war'
            }
        }
        
        stage('Deploy Index.html for Master') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') && env.BRANCH_NAME == 'master' }
            }
            steps {
                sh 'cp path/to/master/index.html path/to/tomcat/webapps/ROOT/'
            }
        }
        
        stage('Deploy Index.html for QA') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') && env.BRANCH_NAME == 'qa' }
            }
            steps {
                sh 'cp path/to/qa/index.html path/to/tomcat/webapps/ROOT/'
            }
        }
    }
}
