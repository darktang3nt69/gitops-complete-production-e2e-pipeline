pipeline{
    agent {
        label 'orange-pi'
    }

    environment{
        APP_NAME = "complete-production-e2e-pipeline-project"
    }

    stages {
        stage('clean'){
            steps{
                // clean workspace
                cleanWs() 
            }
        }

        stage('Checkout from SCM'){
            steps{
                // Clone the main branch of the repo
                checkout scm
            }
        }

        stage('Update deployment file'){
            steps{
                sh """
                echo "Before Deployment:"
                cat deployment.yaml
                sed -i "s/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g" deployment.yaml
                echo "After Deployment:"
                cat deployment.yaml
                """
            }
        }
        stage('Push the changes to scm'){
            steps{
                sh """
                git add .
                git commit -m "Jenkins CD: updated image"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-creds')]) {
                    sh "git push -u origin HEAD:master"
            }
        }
    }
}
}