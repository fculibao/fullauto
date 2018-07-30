pipeline {
    agent any
    tools {
        maven 'maven-3.5.4'
    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging'){
            steps {
                build job: 'webapp-deploy-to-staging'
            }
        }
        stage('Deploy to Production'){
             steps{
                 timeout(time:5, unit:'DAYS'){
                     input message:'Approve PRDUCTION Deployment'
                 }

                 build job: 'webapp-deploy-to-production'
             }
             post {
                 success{
                     echo 'Code deployed to Production'
                 }
                 failure {
                     echo 'Deployment Failed.'
                 }
             }
        }
    }
}
