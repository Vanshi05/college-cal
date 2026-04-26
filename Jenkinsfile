pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling the latest code from the repository...'
                checkout scm
            }
        }

/*stage('SonarQube Code Analysis') {
    agent {
        docker { 
            image 'sonarsource/sonar-scanner-cli' 
            args '--entrypoint=""' 
        }
    }
    steps {
        echo 'Running Analysis...'
        sh 'sonar-scanner -Dsonar.host.url=http://host.docker.internal:9000 -Dsonar.login=sqa_aaeec05ed363353e3528735441f1cf0f386a43de'
    }
}
*/

        stage('Build Docker Image') {
            steps {
                echo 'Building the Docker image for collegia-cal...'
                // Jenkins will now pass this command to your Windows Docker Engine!
                sh 'docker build -t collegia-frontend:latest .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying the new image to the Kubernetes cluster...'
                sh 'kubectl --kubeconfig=/var/jenkins_home/.kube/config --insecure-skip-tls-verify apply -f k8s/'
                
                echo 'Checking deployment status...'
                sh 'kubectl --kubeconfig=/var/jenkins_home/.kube/config --insecure-skip-tls-verify rollout status deployment/collegia-frontend-deployment'
            }
        }
    }
}