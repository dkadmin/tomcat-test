pipeline {
    agent any


    parameters {
         string(name: 'tomcat_staging', defaultValue: '3.111.245.213', description: 'Staging Server')
       //  string(name: 'tomcat_prod', defaultValue: '13.57.204.205', description: 'Production Server')
    }


stages{

       stage ("Git Checkout"){
            steps{
                git credentialsId: 'github', url: 'https://github.com/dkadmin/tomcat-project.git'
            }
        }
        stage('Build'){
            steps {
                sh 'mvn clean package'
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
                        sh "echo y | pscp -i /home/ubuntu/.ssh/id_ed25519 /var/lib/jenkins/workspace/tomcat-deployment/target/*.war ubuntu@${params.tomcat_staging}:/opt/tomcat/webapps/"

                    }
                }

//                stage ("Deploy to Production environment"){
 //                   steps {
 //                       sh "echo y | pscp -i C:\\Users\\grvtr\\Desktop\\Project\\AlternativeFiles\\Redis-Key.ppk C:\\Users\\grvtr\\Desktop\\Project\\AlternativeFiles\\*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
 //                   }
 //               }
            }
        }
    }
}
