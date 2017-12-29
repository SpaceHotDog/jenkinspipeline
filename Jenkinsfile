pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '/home/usr1/Downloads/apache-tomcat-8.5.24-staging/', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '/home/usr1/Downloads/apache-tomcat-8.5.24-prod/', description: 'Production Server')
    }

    triggers {
         pollSCM('H * * * *')
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
                        sh "echo metalero667 | sudo -S cp **/target/*.war ${params.tomcat_dev}webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "echo metalero667 | sudo -S cp **/target/*.war ${params.tomcat_prod}webapps"
                    }
                }
            }
        }
    }
}
