pipeline {
    agent any
    environment {
        APP_NAME = "flight-Pred"
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'GitHub', url: 'https://github.com/handeanjali/FlightPricePrediction-CD.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                script {
                    sh """
                       cat deployment.yaml
                       sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                       cat deployment.yaml
                    """
                }
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                script {
                    gitCommitMsg = "Updated Deployment Manifest with image tag: ${params.IMAGE_TAG}"
                    sh """
                       git config --global user.name "handeanjali"
                       git config --global user.email "anjalihande16@gmail.com"
                       git add deployment.yaml
                       git commit -m "${gitCommitMsg}"
                    """
                }
                withCredentials([gitUsernamePassword(credentialsId: 'GitHub', gitToolName: 'Default')]) {
                    sh "git push https://github.com/handeanjali/FlightPricePrediction-CD main"
                }
            }
        }
    }
}
