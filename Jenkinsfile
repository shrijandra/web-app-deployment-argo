pipeline {
    agent any
    environment {
              APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'gitid', url: 'https://github.com/shrijandra/web-app-deployment-argo.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh '''
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                '''
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh '''
                   git config --global user.name "shrijandra"
                   git config --global user.email "er.shrijandra@yahoo.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                '''
                withCredentials([gitUsernamePassword(credentialsId: 'gitid', gitToolName: 'Default')]) {
                  sh "git push https://github.com/shrijandra/web-app-deployment-argo.git main"
                }
            }
        }
      
    }
}
