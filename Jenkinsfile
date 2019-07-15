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
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''

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
    }
}