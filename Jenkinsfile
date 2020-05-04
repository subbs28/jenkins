pipeline {
    agent any
    
    tools {
        maven 'localMaven'
    }

    parameters {
        string(name: 'tomcat_dev', defaultValue: '34.229.98.50', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '18.206.95.157', description: 'Production Server')
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
        stage('Deployments'){
            parallel {
                stage ('Deploy to staging') {
                    steps {
                        sh "scp -i /Users/shubhankarjadhav/Desktop/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                stage ('Deploy to production') {
                    steps {
                        sh "scp -i /Users/shubhankarjadhav/Desktop/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}