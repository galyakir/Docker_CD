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

         // deploy application to staging
        stage ('staging - deploy') {
            steps {
             withAWS(region:'us-east-1',credentials:'awsCredentials') {
              s3Download bucket: 'web-app-bucket-gal/staging', file: "inventory", path: "inventory"
              s3Download bucket: 'web-app-bucket-gal/staging', file: "vars.yml", path: "vars.yml"
               }
               sh 'ansible-playbook deploy.yml'
            }
        }

        // deploy approval
        stage('Deploy approval'){
           steps{
                input "Deploy to prod?"
           }
        }

        // deploy application to prod
        stage ('prod - deploy') {
            steps {
             withAWS(region:'us-east-1',credentials:'awsCredentials') {
              s3Download bucket: 'web-app-bucket-gal/prod', file: "inventory", path: "inventory",  force: true
              s3Download bucket: 'web-app-bucket-gal/prod', file: "deploy.yml", path: "vars.yml",  force: true
               }
               sh 'ansible-playbook deploy.yml'
            }
        }
    }
}