pipeline {
    agent any
    stages {
    // Building Docker images
        stage('ecr push') {
            steps {     
		    sh 'aws ecr get-login-password --region ap-south-1 | sudo docker login --username AWS --password-stdin 184053799244.dkr.ecr.ap-south-1.amazonaws.com'
	        sh 'sudo docker build -t jenkins-ecr:v${BUILD_NUMBER} -t nikki8:latest .'
	        sh 'sudo docker tag jenkins-ecr:v${BUILD_NUMBER} 184053799244.dkr.ecr.ap-south-1.amazonaws.com/nikki8:v${BUILD_NUMBER}'
		    sh 'sudo docker tag jenkins-ecr:v${BUILD_NUMBER} 184053799244.dkr.ecr.ap-south-1.amazonaws.com/nikki8:latest'
	        sh 'sudo docker push 184053799244.dkr.ecr.ap-south-1.amazonaws.com/jenkins-ecr:v${BUILD_NUMBER}'
		    sh 'sudo docker push 184053799244.dkr.ecr.ap-south-1.amazonaws.com/nikki8:latest'
            }
        }
   

       stage('K8S Deploy') {
        steps{   
            script {
                withKubeConfig([credentialsId: 'K8S', serverUrl: 'http://65.0.89.21:8080/']) {
                sh ('kubectl apply -f  eks-deploy-k8s.yaml')
                }
            }
        }
       }
    }
}
