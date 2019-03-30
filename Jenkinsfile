pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost:9080', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:8090', description: 'Production Server')
    } 

    triggers {
         pollSCM('H * * * *') 
     }

steps{
        
  stages {
    stage('Build'){
        
       node {
            
          withMaven(
        // Maven installation declared in the Jenkins "Global Tool Configuration"
        maven: 'LPT-Maven',
        // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
        // Maven settings and global settings can also be defined in Jenkins Global Tools Configuration
        mavenSettingsConfig: 'my-maven-settings',
        mavenLocalRepo: '.repository') {
         //       sh '/Users/wales_adepejuhotmail.com/Documents/apache-maven-3.5.2/bin/mvn clean package'
                sh 'mvn clean package'
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
}
}
}
