pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost:9080', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:8090', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') 
     }

stages{
        stage('Build'){
            steps {
                sh '/Users/wales_adepejuhotmail.com/Documents/apache-maven-3.5.2/bin/mvn clean package'
            }
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
