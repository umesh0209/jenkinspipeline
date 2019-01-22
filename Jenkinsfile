pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.191.33.29', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.225.7.180', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
                    '/c/Program Files/Git/usr/bin/sh' 'mvn clean package'
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
                        '/c/Program Files/Git/usr/bin/sh' "scp -i /e/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        '/c/Program Files/Git/usr/bin/sh' "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}