pipeline {
    agent any 
    tools{
        maven 'Maven3'
    //   # jdk ''
    }
    stages{
        stage('git checkout'){ 
            steps{
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Adarsh623/javak8sdeploy.git']])
            } //checkout:checkout from version control (pipeline script)
        }
        stage('maven build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('docker build image'){
            steps{
                sh ' docker build -t adarsh623/devops-integration .'
            }
        }
        stage('docker push to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerhubpassword')]) { //withCredentials:Bind credentials to variables (pipelinescript)
    // some block 
                    sh 'docker login -u adarsh623 -p${dockerhubpassword}'
}
                    sh 'docker push adarsh623/devops-integration '
                }
            }
        }
        stage('deployment to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'k8sconfigpassword') //KubernetesDeploy: Deploy to kubernetes (pipeline_script)
                }
            }
        }
    }
}
