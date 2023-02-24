pipeline {
    agent { label 'final-proj-slave' }
    stages {
        stage('build') {
            steps {
                script {
                    sh   """
                        docker build -t gcr.io/moelzedy/app:${BUILD_NUMBER} .
                        docker push gcr.io/moelzedy/app${BUILD_NUMBER}
                        echo ${BUILD_NUMBER} > ../proj-build-number.txt
                    """
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    sh    """
                        kubectl apply -f deployment.yaml
                        kubectl apply -f service.yaml
                    """
                }
            }
        }
    }
}