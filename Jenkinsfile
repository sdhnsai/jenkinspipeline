pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archive Artifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}
