pipeline{
    agent{
        label "jenkins-agent"
    }
    environment{
        APP_NAME = "ci-cd-e2e"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }

        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Baretsky/ci-cd-e2e-deployment'
            }
        }

        stage("Update deployment tags"){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i "s|baretsky24/${APP_NAME}:.*|baretsky24/${APP_NAME}:${IMAGE_TAG}|g" deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to git"){
            steps{
                sh """
                    git config --global user.name "Baretsky"
                    git config --global user.email "dulon.dara24@gmail.com"
                    git add deployment.yaml
                    git commit -m "Update Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
                    sh "git push https://github.com/Baretsky/ci-cd-e2e-deployment main"
                }
            }
        }
    }
}
