pipeline {
    agent { label "Jenkins-Slave-Agent-Node-Label" }
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
                    git branch: 'master', credentialsId: 'Github-Credentials', url: 'https://github.com/princewillopah/DevOps-CI-withJenkins-CD-withArgoCD-reg-app-gitops-repo'
               }
        }

        // stage("Update the Deployment Tags") {
        //     steps {
        //         sh """
        //            cat deployment.yaml
        //            sed -i "s/\${APP_NAME}.*/\${APP_NAME}:\${IMAGE_TAG}/g" deployment.yaml
        //            sed -i "s/$${APP_NAME}.*/$${APP_NAME}:$${env.IMAGE_TAG}/g" deployment.yaml
        //            cat deployment.yaml
        //         """
        //     }
        // }
             stage("Update the Deployment Tags") {
                steps {
                    sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                    """
                }
            }
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "princewillopah"
                   git config --global user.email "princewillopah@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'Github-Credentials', gitToolName: 'Default')]) {
                  sh "git push https://github.com/princewillopah/DevOps-CI-withJenkins-CD-withArgoCD-reg-app-gitops-repo master"
                }
            }
        }
      
    }
}
