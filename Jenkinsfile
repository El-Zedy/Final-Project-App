pipeline {
    agent { label 'final-proj-slave' }
    stages {
        stage('build') {
            steps {
                script {
                    sh   """
                        docker build -t gcr.io/moelzedy/app:${BUILD_NUMBER} .
                        docker push gcr.io/moelzedy/app:${BUILD_NUMBER}
                        echo ${BUILD_NUMBER} > ../proj-build-number.txt
                    """
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    sh    """
                        export BUILD_NUMBER=\$(cat ../proj-build-number.txt)
                        mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                        cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                        rm -f Deployment/deploy.yaml.tmp
                        kubectl create ns app-ns
                        kubectl apply -f Deployment
                    """
                }
            }
        }
    }
}
