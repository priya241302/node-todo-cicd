pipeline {
    agent  any
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }
    
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/priya241302/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build and Test"){
            steps{
                sh "docker build . -t node-app-test-new"
            }
        }

         stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Todo -Dsonar.projectKey=Todo "
                }
            }
        }
         stage("Docker Build & Push"){
            steps{
                script{
                        withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') {
                  
                        
    
                             sh "docker tag node-app-test-new priya247/node-app-test-new:latest "
                             sh "docker push priya247/node-app-test-new:latest "
                    
                    }
                }
            }
        }

        
        
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
