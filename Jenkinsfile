pipeline {
    agent { label 'final-proj-slave' }
    stages {
        stage('build') 
        {
            steps 
            {
                script 
                {
                    withCredentials([usernamePassword(credentialsId: 'Dockerhub-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                    {
                        sh   """
                            docker login -u $USERNAME -p $PASSWORD
                            docker build -t moelzedy/app:${BUILD_NUMBER} .
                            docker push moelzedy/app:${BUILD_NUMBER}
                            echo ${BUILD_NUMBER} > ../proj-build-number.txt
                        """
                    }
                }
            }
        }
        stage('deploy')
        {
            steps 
            {
                    sh """
                       kubectl get ns

                    """
                }
            }
        }
    }
