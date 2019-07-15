pipeline{
    agent any
    tools { 
        maven 'Maven3' 
        jdk 'JAVA8' 
    }
    stages{
        stage('Build'){
            steps{
                script {
                    tagName = 'v1.2'
                    }
                sh '''
                    mvn clean package
                '''
               
            }
            post{
                success{
                    echo 'Now Archiving started....'
                    archiveArtifacts artifacts: '**/target/*.war'

                     withCredentials([usernamePassword(credentialsId: 'git-pass-credentials-ID', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {                        
                        sh('echo "Tag Name is ${tagName}"')
                        sh('echo "User Name is ${GIT_USERNAME}"')
                        sh("git tag -a ${tagName} -m 'Tagging release build with name of ${tagName}'")
                        sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@<REPO> ${tagName}')
                    }
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