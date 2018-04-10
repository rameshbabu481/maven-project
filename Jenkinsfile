pipeline {
    agent any
	
	
    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.220.32.129', description: 'Staging Server')
		 string(name: 'tomcat_prod', defaultValue: '18.217.29.218', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

    stages {
         stage('Build'){
             steps {
                sh '/usr/local/maven/current/mvn clean package'
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
                        sh "scp -i /Users/Nanda Gopal/tomcat-test.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/Nanda Gopal/tomcat-test.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
                
            }
        }
    }
}
