pipeline {
    agent any
    tools{
        maven 'Maven-3.9.3'
    }
    environment{
        DOCKERHUB_CREDENTIALS=credentials('635f714f-0de1-4726-b5cd-a6762965dc07')
    }

    stages{
            stage('Build Maven'){
                steps{
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Neysho/Spring-Boot-CRUDProject.git']])
                    sh 'mvn clean install'
                }
            }
             stage('Build docker image'){
                        steps{
                            script{
                                sh 'docker build -t neysho/springboot-crud:$BUILD_NUMBER .'
                            }
                        }
                    }
             stage('Login to Docker Hub')
             {
                steps{
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
             }
             stage('Push image to Docker Hub')
             {
                steps{
                    sh 'docker push neysho/springboot-crud:$BUILD_NUMBER'
                }
             }

          }
          post {
            always {
                sh 'docker logout'
            }
            }
    }