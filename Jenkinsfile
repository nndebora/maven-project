pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost', description: 'Production Server')
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
                        bat "cp webapp/target/*.war F:/pessoal/cursos/jenkins/apache-tomcat-9.0.5-staging/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        
                        bat "cp webapp/target/*.war F:/pessoal/cursos/jenkins/apache-tomcat-9.0.5-production/webapps"
                    }
                }
            }
        }
    }
}