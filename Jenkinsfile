pipeline {
    agent any
    tools {
        maven 'maven-3.5.4'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.173.49.29', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.232.168.226', description: 'Production Server')
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
                        sh "scp -i /Users/ferdieculibao/Documents/CONN/inan01.pem /Users/ferdieculibao/.jenkins/workspace/FullyAutomated/target/*.war ec2-user@${params.tomcat_dev}:/opt/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/ferdieculibao/Documents/CONN/inan01.pem /Users/ferdieculibao/.jenkins/workspace/FullyAutomated/target/*.war ec2-user@${params.tomcat_prod}:/opt/tomcat/webapps"
                    }
                }
            }
        }
    }
}

