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

                     sh '''
                        git config user.email praveenkumar.myl@gmail.com
                        git config user.name src-praveen
                    '''   

                     withCredentials([usernamePassword(credentialsId: 'git-pass-credentials-ID', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {                        
                        sh('echo "Tag Name is ${tagName}"')
                        sh('echo "User Name is ${GIT_USERNAME}"')
                        sh("git tag -a ${tagName} -m 'Tagging release build with name of ${tagName}'")
                        sh('git push https://${GIT_USERNAME}@github.com/src-praveen/maven-project.git ${tagName}')
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