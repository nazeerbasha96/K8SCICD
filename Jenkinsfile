pipeline {
    agent any
    tools {
        maven 'mvn'
        terraform 'tf'
    }
     environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    stages {
        stage ('Clone Code') {
            steps {
                git url: "https://github.com/nazeerbasha96/K8S.git" , branch: "master"
            }
        // }
        // stage ('Create a k8s cluster' ){
        //     steps{
        //         sh '''
        //             terraform -chdir=Terraform/Golbal/ init
        //             terraform -chdir=Terraform/Golbal/ plan
        //             terraform -chdir=Terraform/Golbal/ apply --auto-aprove
        //             terraform -chdir=Terraform/Golbal/ output  "db_address" > dbHosts
        //         '''
        //     }
        // }
        //  stage ('config db url'){
        //     steps {
        //         sh ''' 
        //         dbHost=$(cat dbHosts)
        //         sed -i "s|dbhost|$dbHost|g" urotaxi-configmap.yml
                
        //         mysql -uroot -pwelcome#123 -h 
        //     '''
        //     }
        // }
        // stage ('build code'){
        //     steps {
        //         sh ' mvn  clean verify'
        //     }
        // }
        stage('bulid Image'){
            steps {
                echo"building the docker image"
                sh "docker build -t urotaxi ."
            }
        }
        stage ('Push Image'){
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker image tag urotaxi ${env.dockerhubuser}/urotaxi:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}" 
                sh "docker push ${env.dockerhubuser}/urotaxi:latest"

                }
                
            }
        }
    }
}