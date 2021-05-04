pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'jdk'
    }

    parameters {
         string(name: 'staging', defaultValue: '34.219.120.54', description: 'Staging Server')
         string(name: 'prod', defaultValue: '18.237.74.144', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/laddu/jenkins.pem **/target/*.war ec2-user@${params.staging}:/home/ec2-user/apache-tomcat-8.5.65/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/laddu/jenkins.pem **/target/*.war ec2-user@${params.prod}:/home/ec2-user/apache-tomcat-8.5.65/webapps"
                    }
                }
            }
        }
    }
}
