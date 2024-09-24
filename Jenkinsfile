pipeline {
    agent any
    tools {
        nodejs 'NodeJS 22.9.0'
    }

    environment {
        DEPLOY_HOOK_URL = "https://api.render.com/deploy/srv-crou10jv2p9s739aibi0?key=LC52FSzUbZA"
    }
   
    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'git project', url: 'https://github.com/C22-max/gallery.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                     sh 'npm install mocha'
                      sh 'npm install chai'
                }
            }
        }
        // Uncomment these stages if you want to include tests and deployment steps
        stage('Run Tests') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }
        /*

        stage('Deploy to Render') {
            steps {
                script {
                    sh 'node server' // Server entry point
                }
            }
        }
        */

        stage('Deploy to Render122') {
            steps {
                script {
                    def response = sh(script: """
                        curl -X POST ${DEPLOY_HOOK_URL}
                    """, returnStdout: true).trim()
                   
                    echo "Deployment Response: ${response}"
                }
            }
        }

        stage('Notify Slack') {
            steps {
                slackSend(
                    botUser: true,
                    channel: 'C07NRNV75N0',
                    color: '#0000FF',
                    message: "Deployment successful! Build ID - ${env.BUILD_ID}. Check the deployed site: https://gallery-8tyg.onrender.com",
                    teamDomain: 'christie',
                    tokenCredentialId: 'slack-2'
                )
            }
        }
    }

    post {
        failure {
            echo "Pipeline failed"
            // Uncomment below to send failure notification email
            // mail to: 'catemirobe@gmail.com',
            //      subject: "Build Failed: ${currentBuild.fullDisplayName}",
            //      body: "Something is wrong with ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }

        success {
            echo "Pipeline passed"
            // Uncomment below to send success notification email
            // mail to: 'catemirobe@gmail.com',
            //      subject: "Build Successful: ${currentBuild.fullDisplayName}",
            //      body: "Build successful for ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
    }
}
