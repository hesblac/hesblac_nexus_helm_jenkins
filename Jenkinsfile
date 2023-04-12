pipeline {

    agent any
    
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

    }
}