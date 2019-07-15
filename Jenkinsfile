pipeline{
    agent any
   // properties([parameters([string(defaultValue: '', description: '', name: 'tagName', trim: true)])])
    parameters {
        booleanParam(name: 'release', defaultValue: false, description: 'release')
       // string(name: 'tagName', defaultValue: '', description: 'Release Tag name')
    }
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
        }

        stage('Release Stage'){
                when{
                    expression {
                        params.release == true
                    }
                    input {
                        message  "Enter the tag name"
                        parameters: [
                            string(name: 'tagName',defaultValue : '',description: 'Enter the tag name')
                        ]
                    }
                }
                // if(params.release){
                //     input {
                //         message  "Enter the tag name"
                //         parameters: [
                //             string(name: 'tagName',defaultValue : '',description: 'Enter the tag name')
                //         ]
                //     }
                // }
                steps{
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
                        sh("git tag -a ${params.tagName} -m 'Tagging release build with name of ${params.tagName}'")
                        sh("git push https://${GIT_USERNAME}:${GIT_PASSWORD}@${REPO} ${params.tagName}")
                    } 
            }  
            
            // stage('Deploy to Staging'){
            //     steps{
            //         build job: 'deploy-to-staging'
            //     }
            // }
        } 

        post{
            success{
                echo 'Now Archiving started....'
                archiveArtifacts artifacts: '**/target/*.war'        
                    
            }
        }


    }       
        
}
