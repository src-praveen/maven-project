pipeline{
    agent any
    stages{
        stage('Build'){
            steps{
                scripts{
                    mavenHome = tool name: 'Maven3', type: 'maven'
                }
                echo "maven home ${mavenHome}"
                sh "'${mavenHome}/bin/mvn' clean install"
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