pipeline{
    agent any
    tools { 
        maven 'Maven3' 
        jdk 'JAVA8' 
    }
    stages{
        stage('Build'){
            steps{
                sh '''
                    mvn clean package
                '''
               
            }
            post{
                success{
                    echo 'Now Archiving started....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging'){
            steps{
                build job: 'deploy-to-staging'
            }
        }
    }
}