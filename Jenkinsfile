// Global variables
def gv

pipeline {
    agent any
    environment {
        NEW_VERSION = '1.3.4'
        // SERVER_CREDENTIALS = credentials('aws-web-server')
    }
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: 'version to deploy')
        booleanParam(name: 'executeTest', defaultValue: true, description: 'Execute Test Script')
    }
    stages {
        stage('init') {
            steps {
                script {
                    gv = load"build-script.groovy"
                }
            }
        }
        stage('Build') {
            when {
                expression {
                   JOB_NAME == 'jenkinsfile-testing' 
                }
            }
            steps {
                echo 'Building'
                echo "branch: ${BRANCH_NAME}"
                script {
                    gv.buildApp()
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    gv.testApp()
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
                script {
                    gv.deployApp()
                }
            }
        }
    }
}