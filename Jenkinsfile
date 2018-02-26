pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost:8089', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:9009', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
 
stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                        bat "winscp -i **/target/*.war {params.tomcat_dev}"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i **/target/*.war {params.tomcat_prod}"
                    }
                }
            }
        }
    }
}