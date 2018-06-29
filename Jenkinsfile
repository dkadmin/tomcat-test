pipeline {
    agent any

    tools {
          maven 'localMaven'
          jdk 'localJDK'
    }
    parameters {
         string(name: 'tomcat_staging', defaultValue: '54.153.121.139', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.57.204.205', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving starts here...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging environment'){
                    steps {
                        bat "pscp -i StrictHostKeyChecking=no C:\\Users\\grvtr\\Desktop\\Project\\AlternativeFiles\\Redis-Key.pem C:\\Program Files (x86)\\Jenkins\\workspace\\FullyAutomatedPipeline\\webapp\\target\\*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production environment"){
                    steps {
                        bat "pscp -i StrictHostKeyChecking=no C:\\Users\\grvtr\\Desktop\\Project\\AlternativeFiles\\Redis-Key.pem C:\\Program Files (x86)\\Jenkins\\workspace\\FullyAutomatedPipeline\\webapp\\target\\*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}