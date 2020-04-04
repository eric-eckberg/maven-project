pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                build job: 'mavin-project-staging'
            }
        }

        stage('Deploy to Production') {
            steps {
                timeout( time: 5, unit: 'DAYS' ) {
                    input message: 'Approve PRODUCTION Deployment?'
                }
                build job: 'maven-project-deploy-to-prod'
            }
            post {
                success {
                    echo 'Successfully Deployed to Production'
                }

                failure {
                    echo 'Deployment Failed!'
                }
            }
        }
    }
}