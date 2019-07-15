pipeline{
    agent any
    options(
            [
                parameters(
                    [
                        string(defaultValue: '', description: '', name: 'tagName', trim: true)
                    ])
            ]
        )
    tools { 
        maven 'Maven3' 
        jdk 'JAVA8' 
    }
    stages{
        stage('Build'){
            // input {
            //     message "Tag name of the release"
            //     parameters {
            //         string(name: 'tagName', defaultValue: '0.0.1', description: 'Tag name of the release')
            //     }
            // }
            steps{
                script {
                   // tagName = 'v1.2.5'
                    REPO = 'github.com/src-praveen/maven-project.git'
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
                        sh '''
                            git tag -l
                            git remote show origin
                        '''
                        sh('echo "User Name is ${GIT_USERNAME}"')
                        sh("git tag -a ${tagName} -m 'Tagging release build with name of ${tagName}'")
                        sh("git push https://${GIT_USERNAME}:${GIT_PASSWORD}@${REPO} ${tagName}")
                    }
                }
            }
        }
        // stage('Deploy to Staging'){
        //     steps{
        //         build job: 'deploy-to-staging'
        //     }
        // }
    }
}