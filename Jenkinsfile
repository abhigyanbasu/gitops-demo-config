pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = "abhigyanbasu"
        APP_NAME = "gitops-demo-app"
        REGISTRY_CREDS = 'dockerhub'
        }
    //parameters {
      //  string(name: 'IMAGE_TAG', description: 'enter env value')
    //}
    stages {
        stage('Cleanup Workspace'){
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){
            steps {
                git credentialsId: 'github', 
                url: 'https://github.com/abhigyanbasu/gitops-demo-config.git',
                branch: 'master'
            }
        }
	    stage('Updating Kubernetes deployment file'){
            steps {
                sh "cat deployment.yml"
                sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml"
                sh "cat deployment.yml"
            }
        }
		 stage('Push the changed deployment file to Git'){
            steps {
                script{
                    sh """
                    git config --global user.name "vikram"
                    git config --global user.email "vikram@gmail.com"
                    git add deployment.yml
                    git commit -m 'Updated the deployment file' """
                    withCredentials([usernamePassword([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) 
                    {
                        sh "git push https://github.com/abhigyanbasu/gitops-demo-config.git master "
                        //sh "git push https://$user:$pass@github.com/abhigyanbasu/gitops-demo-config.git master"
                    }
                }
            }
		 }
    }
}
