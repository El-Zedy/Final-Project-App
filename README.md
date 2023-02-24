# ITI-Final-Project-App :curly_loop:
Build and Deploy simple web on `GKE` private cluster using `docker` and `kubernetes` automated through `jenkins` pipeline
## Prerequisites
- Linux system
- Jenkins-slave up and running 
- Docker
## Getting Started
- To use this project, you will need to clone the repository to your local machine using the following command:
    
      git clone https://github.com/El-Zedy/Final-Project-App
      
- Jenkinsfile 
    - have to stages one for `build` and another for `deploy`
        
        - build stage:
              
              withCredentials([usernamePassword(credentialsId: 'Dockerhub-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                    {
                        sh   """
                            docker login -u $USERNAME -p $PASSWORD
                            docker build -t moelzedy/app:${BUILD_NUMBER} .
                            docker push moelzedy/app:${BUILD_NUMBER}
                            echo ${BUILD_NUMBER} > ../proj-build-number.txt
                        """
                    }
            
            - login at dockerhub 
            - bulid our app image from our Dockerfile
            - push our image to dockerhub to use it at our deployment later
            
         - deploy stage:
            
                def namespace = "app-ns"
                      def namespaceExists = sh(returnStdout: true, script: "kubectl get ns | grep ${namespace} | wc -l").trim()
                      if (namespaceExists == "1") 
                      {
                          echo "Namespace ${namespace} already exists, skipping creation step."
                      } else {
                          sh "kubectl create namespace ${namespace}"
                      }
                      sh    """
                          export BUILD_NUMBER=\$(cat ../proj-build-number.txt)
                          mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                          cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                          rm -f Deployment/deploy.yaml.tmp
                          kubectl apply -f Deployment
                      """
            - check if app-ns namespace existing or no, if yes skip this point and if no create it 
            - apply our Deployment yml files
  
- Create pipline at jenkins ui
  
    - add piple metadata
    - create credentials for dockrhub
    - configure SCM
    - start buliding our pipline
    
- check resource created by pipline
    
      kubectl get all -n app-ns
      
- get service external-ip to access our app from browser

  Hit `EXTERNAL_IP` on your browser
  
- **Congratulation our web application is now available for users! :rocket::rocket:**
## Contributing

Contributions to this project are welcome and appreciated. To contribute, please follow these steps:

1. Fork the repository to your own account.
2. Create a new branch for your changes.
3. Make your changes and commit them to your branch.
4. Create a pull request to merge your changes into the main branch.

## Acknowledgements
We would like to acknowledge the following individuals and resources for their contributions to this project:

- Our instructors, for providing guidance and support throughout the project.
- The Google Cloud Platform documentation, which served as a valuable resource for understanding the various GCP services and how they can be used together.
- Jenkins and Kubernetes documentation, which provided helpful guidance on building our resources.
- We would also like to thank our classmates and peers for their feedback and contributions to this project.

## Contact
If you have any questions or suggestions regarding this project, please contact the project owner at muhammadhassanelzedy@gmail.com .

























