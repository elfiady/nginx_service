pipeline {
environment { // Declaration of environment variables
DOCKER_ID = "elfiadylclc" // replace this with your docker-id
DOCKER_IMAGE_CAST = "datascientestcast"
DOCKER_IMAGE_MOVIE = "datascientestmovie"
DOCKER_TAG = "v.${BUILD_ID}.0" // we will tag our images with the current build in order to increment the value by 1 with each new build
}
agent any // Jenkins will be able to select all available agents
stages {

         stage(' Docker run ngnix '){
            steps {
                            script {
                            sh '''
                            docker rm -f nginx
                            docker run -d -p 8088:8080 --volume ./nginx_config.conf:/etc/nginx/conf.d/default.conf --name nginx nginx:latest
                            sleep 10
                            '''
                            }
                }

        }       

        stage('Test Acceptance'){ // we launch the curl command to validate that the container responds to the request
            steps {
                    script {
                    sh '''
                    curl localhost
                    '''
                    }
            }

        }      

        stage('Deploiement en dev nginx app'){
            environment
                        {
                        KUBECONFIG = credentials("config") // we retrieve  kubeconfig from secret file called config saved on jenkins
                        }
                            steps {
                                script {
                                sh '''
                                rm -Rf .kube
                                mkdir .kube
                                ls
                                cat $KUBECONFIG > .kube/config
                                cp nginx/values.yaml values.yml
                                cat values.yml
                                sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                                helm upgrade --install nginx nginx --values=values.yml --namespace dev
                                '''
                                }
                            }
        }                    

          stage('Deploiement en qa nginx app'){
            environment
                        {
                        KUBECONFIG = credentials("config") // we retrieve  kubeconfig from secret file called config saved on jenkins
                        }
                            steps {
                                script {
                                sh '''
                                rm -Rf .kube
                                mkdir .kube
                                ls
                                cat $KUBECONFIG > .kube/config
                                cp nginx/values.yaml values.yml
                                cat values.yml
                                sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                                helm upgrade --install nginx nginx --values=values.yml --namespace qa
                                '''
                                }
                            }
          }     

          stage('Deploiement en staging nginx app'){
            environment
                        {
                        KUBECONFIG = credentials("config") // we retrieve  kubeconfig from secret file called config saved on jenkins
                        }
                            steps {
                                script {
                                sh '''
                                rm -Rf .kube
                                mkdir .kube
                                ls
                                cat $KUBECONFIG > .kube/config
                                cp nginx/values.yaml values.yml
                                cat values.yml
                                sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                                helm upgrade --install nginx nginx --values=values.yml --namespace staging
                                '''
                                }
                            }
        }    

        stage('Deploiement en prod nginx app'){
            environment
                        {
                        KUBECONFIG = credentials("config") // we retrieve  kubeconfig from secret file called config saved on jenkins
                        }
                            steps {
                                script {
                                sh '''
                                rm -Rf .kube
                                mkdir .kube
                                ls
                                cat $KUBECONFIG > .kube/config
                                cp nginx/values.yaml values.yml
                                cat values.yml
                                sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                                helm upgrade --install nginx nginx --values=values.yml --namespace prod
                                '''
                                }
                            }
        }    
}
}
