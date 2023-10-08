pipeline{
    agent any
    stages{
        stage('clean work space'){
            steps{
                deleteDir()
            }
        }
        stage('git checkout'){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/mukeshr-29/devops-project-9.git'
            }
        }
        stage('unit testing'){
            steps{
                sh 'mvn test'
            }
        }
        stage('integration test'){
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('build application artifact'){
            steps{
                sh 'mvn clean install'
            }
        }
    }
}