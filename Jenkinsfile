pipeline{
    agent {
        label 'orange-pi'
    }

    // environment{
    //     IMAGE_NAME = "${IMAGE_NAME}"
    // }

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
                sed -i "s|image: .*|image: ${IMAGE_TAG}|" deployment.yaml
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