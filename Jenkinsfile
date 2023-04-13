pipeline {

    agent any
    environment{

        VERSION = "${env.BUILD_ID}"
    }
    
    stages{

        stage('Sonar quality check') {

            agent{

                docker {
                    image 'maven'
                }
            }
            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'sonartoken') {
                    sh 'mvn clean package sonar:sonar'
                }
                }
            }
        }
        stage('quality gate status') {

            steps{

                script{

                    waitForQualityGate abortPipeline: false, credentialsId: 'sonartoken'
                }
            }
        }
        stage('docker build and push to nexus repo'){

            steps{

                script{
                    
                    withCredentials([string(credentialsId: 'nexus_passwd', variable: 'nexus_cred')]) {
                    sh """
                    docker build -t 54.226.64.12:8083/springapp:$[VERSION] .
                    docker login -u admin -p $nexus_cred 54.226.64.12:8083
                    docker push  54.226.64.12:8083/springapp:$[VERSION]
                    docker rmi  54.226.64.12:8083/springapp:$[VERSION]

                    """

                    }
                }
            }
        }

    }
}