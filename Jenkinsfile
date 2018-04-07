pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.219.210.58', description: 'Staging Server')
		 string(name: 'tomcat_prod', defaultValue: '18.218.23.3', description: 'Production Server')
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
