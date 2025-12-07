pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mdaniyalhammad/multi-service-app'
        IMAGE_TAG  = "v${env.BUILD_NUMBER}"
        K8S_DEPLOYMENT = 'simple-app-deploy'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/BSSE23046/Software-Quiz2.git'
            }
        }

        stage('Build Docker Image') {
    steps {
        script {
            // "." changed to "./backend" to point to your folder
            docker.build("${IMAGE_NAME}:${IMAGE_TAG}", "./backend")
        }
    }
}
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1','docker-hub-creds') {
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl set image deployment/${K8S_DEPLOYMENT} \
                backend=${IMAGE_NAME}:${IMAGE_TAG} --record
                """
            }
        }
    }

    post {
        success {
            echo "CI/CD Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}






