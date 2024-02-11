pipeline {
    agent any

    stages {
        stage('Clone repository') {
          steps {
            checkout scm
          }    
       }
        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                            sh "git config user.email awsandazurecloudengineer@gmail.com"
                            sh "git config user.name AWSandAzureandFullStackEngineer"
                            //sh "git switch master"
                            sh "cat deployment.yml"
                            sh "sed -i 's+steven8519/authentication-service.*+steven8519/authentication-service:${DOCKERTAG}+g' deployment.yml"
                            sh "cat deployment.yml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/test-deployment.git HEAD:main"
                        }
                    }
                }
            }
        }
        stage('Remove docker images and containers') {
            steps {
                sh 'docker system prune -a --force'
            }
        }
        stage('Cleanup') {
            steps {
                // Clean up Docker images after build
                cleanWs()
            }
        }  
    }
}
