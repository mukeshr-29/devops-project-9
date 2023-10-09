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
        stage('static code analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarqube'){
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage('quality gate'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube'
                }
            }
        }
        stage('upload jar file to nexus'){
            steps{
                script{
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', 
                            type: 'jar'
                        ]
                    ], 
                    credentialsId: 'nexus',
                    groupId: 'com.example', 
                    nexusUrl: '10.0.0.24:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'devops-project-9-release', 
                    version: '1.0.0'
                }
            }
        }
    }
}