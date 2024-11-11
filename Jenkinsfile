pipeline{
    agent {label "Jenkins-Agent"}
    environment{
        APP_NAME = "register-app-pipeline"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/varaprasad431/projects.git'
            }
        }
        stage("Update the Deployment Tags"){
            steps{
                sh """ 
                cat deployment.yaml
                sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to git"){
            steps{
                sh """
                git config --global user.name "varaprasad431"
                git config --global user.email "varaprasadkatteboina001@gmail.com"
                git add deployment.yaml
                git commit -m "Updated Deployment manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
                    sh "https://github.com/varaprasad431/projects.git"
                }
            }
        }
    }
}
