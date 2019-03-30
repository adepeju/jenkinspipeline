pipeline {
    agent any 
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost:9080', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:8090', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('H * * * *') 
     }

stages{
        stage('Build'){
            steps {
              withMaven(maven: 'LPT-Maven', mavenSettingsConfig: 'mvn-setting-xml') {
                sh 'mvn clean package'
    }
}
//                sh 'mvn clean package'
//            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deploy to Staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }
    
        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }
            
                build job: 'Deploy-to-Prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }
                
                failure {
                    echo 'Deployment failed.'
                }    
            }
        }
    }
}
