pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '34.241.149.198', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.243.100.172', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
                        bat "winscp -i \Users\Zsolt_Nyerges\Downloads\AWS\Keypairs\tomcat-demo.pem **/target/*.war centos@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i \Users\Zsolt_Nyerges\Downloads\AWS\Keypairs\tomcat-demo.pem **/target/*.war centos@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}