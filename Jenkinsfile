pipeline{
    agent any
    parameters{
        choice(name: 'action',choices: 'create\ndestroy\ndestroyekscluster', description: 'Create/Update or destroy the eks cluster')
        string(name: 'cluster', defaultValue: 'project-cluster',description: 'Eks cluster name')
        string(name: 'regions', defaultValue: 'us-east-1',description: 'Eks cluster region')
    }
    environment{
        ACCESS_KEY = credentials('aws_access_key_id')
        SECRET_KEY = credentials('aws_secret_access_key_id')
    }
    stages{
        stage("git checkout"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/mukeshr-29/devops-project-9.git'
            }
        }
        stage("eks connect"){
            steps{
                withCredentials([string(credentialsId: 'aws_access_key_id', variable: 'ACCESS_KEY'), string(credentialsId: 'aws_secret_access_key_id', variable: 'SECRET_KEY')]) {
                    sh """
                         aws eks --region ${params.regions} update-kubeconfig --name ${params.cluster}
                     """
                }
            }
        }
        stage("eks deployment"){
            when { expression { params.action == 'create'}}
            steps{
                script{
                    def apply = false
                    try{
                        input message: 'please confirm the apply to innitiate the deployments', ok: 'Ready to apply the config'
                        apply = true
                    }
                    catch(err){
                        apply = false
                        CurrentBuild.result= 'UNSTABLE'
                    }
                    if(apply){
                        sh """ 
                            kubectl apply -f .
                        """
                    }
                }
            }
        }
        stage("delete deployment"){
            when{expression {params.action == 'destroy'}}
            steps{
                script{
                    def destroy = false
                    try{
                       input message: 'please confirm the destroy to delete the deployments', ok: 'Ready to destroy the config' 
                       destroy = true
                    }
                    catch(err){
                        destroy = false
                        CurrentBuild.result= 'UNSTABLE'
                    }
                    if(destroy){
                        sh """
                            kubectl delete -f .
                        """
                    }
                }
            }
        }
    }
}
