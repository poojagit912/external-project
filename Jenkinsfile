pipeline{
    agent any
    environment {
        Image_name = "poojadocker912/external-app:V_${BUILD_ID}"
        docker_user = "poojadocker912"
        docker_pass = "FEWsteps*2018"
    }
    stages{
        stage('dependancy versions'){
            steps{
                sh '''
                    git --version
                    docker --version
                    npm -v
                '''
            }    
        }
        stage('git checkout'){
            steps{
                    git 'https://github.com/poojagit912/external-project.git'
            }    
        }
        stage('git test'){
            steps{
                sh '''
                    echo "install dependencies and test external code ..!"
                    npm install
                   # npm test
                ''' 
            }    
        }
        stage('build'){
            steps{
                sh '''
                    echo "${Image_name}"
                    echo "build and push docker image for external app ..!"
                    docker build . -t ${Image_name}
                    docker login --username ${docker_user} --password ${docker_pass}
                    docker push ${Image_name}
                ''' 
            }    
        }
         stage('deploy'){
            steps{
                sh """
                    gcloud container clusters get-credentials deployment-cluster --zone us-central1-c --project orbital-amulet-282713
                    kubectl set image deployment/events-web events-web=${Image_name}
                 """
                //  #kubectl apply -f kube-config-external.yaml                  
            }    
        }  
    }
}
