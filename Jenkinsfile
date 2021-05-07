pipeline {

    agent {label 'aws'}

    stages {
        // this stage clean the environment
        stage ('checkout') {
            steps{
                cleanWs()
                checkout scm
            }
       }

        // this stage download variables file for terraform from S3
        stage ('S3 download tfvars') {
            steps {
                withAWS(region:'us-east-1',credentials:'awsCredentials') {
                    s3Download bucket: 'web-app-bucket-gal/staging', file: "inventory", path: "inventory"
                    s3Download bucket: 'web-app-bucket-gal/staging', file: "deploy.yml", path: "deploy.yml"
                }
            }
        }

         // deploy application to staging
        stage ('staging - deploy') {
            steps {
                    sh 'ansible-playbook deploy.yml'
            }
        }


    }
}