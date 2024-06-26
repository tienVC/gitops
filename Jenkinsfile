pipeline {
    agent any
    environment {
                  APP_NAME = "multi-client"
    }
    stages {
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     git branch: 'main', credentialsId: 'github', url: 'https://github.com/tienVC/gitops'
             }
         }
         stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat client-deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' client-deployment.yml
                    cat client-deployment.yml
                """
            }
         }
         stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "tienvc"
                    git config --global user.email "vucongtien0311@gmail.com"
                    git add client-deployment.yml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/tienVC/gitops main"
                }
            }
         }
    }
}
