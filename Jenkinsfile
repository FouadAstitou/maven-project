pipeline {
    agent any
 tools {
    maven 'localMAVEN'
 }
parameters {
         string(name: 'tomcat_dev', defaultValue: '18.195.35.248', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.59.245.185', description: 'Production Server')
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
                        sh "scp -i /Users/fouadastitou/Downloads/TomcatStaging **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/fouadastitou/Downloads/TomcatProd **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}