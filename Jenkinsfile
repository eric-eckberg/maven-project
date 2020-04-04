pipeline {
    agent any

    parameters {
        string( name: 'dev_path', defaultValue: '/opt/tomcat/webapps', description: 'Staging Path')
        string( name: 'prod_path', defaultValue: '/opt/tomcat-prod/webapps', description: 'Production Path')
    }

    triggers {
        pollSCM('* * * * *')
    }

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
        stage('Deployments') {
            parallel {
                stage ('Deploy to Staging') {
                    steps {
                        sh "sudo cp **/target/*.war {params.dev_path}"
                    }
                }
                stage ('Deploy to Production') {
                    steps {
                        sh "sudo cp **/target/*.war {params.prod_path}"
                    }
                }                
            }
        }
    }
}