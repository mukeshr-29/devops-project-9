pipeline{
    agent any
    stages{
        stage('clean work space'){
            steps{
                cleanWS()
            }
        }
        stage('git checkout'){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/mukeshr-29/devops-project-9.git'
            }
        }
    }
}