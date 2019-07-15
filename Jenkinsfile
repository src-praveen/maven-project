pipeline{
    agent any
    stages{
        stage('Build'){
            def mavenHome
            steps{
                mavenHome = tool name: 'Maven3', type: 'maven'
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