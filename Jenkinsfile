pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "mandeep1690/servicea"
 	dockerfile= "Dockerfile-small"   
}
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'main'
            }
            steps {
                script {
		    sh 'cd serviceA'
                    app = docker.build(DOCKER_IMAGE_NAME, "-f ${dockerfile} .")
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockercred') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'main'
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)
                //implement Kubernetes deployment here
        	kubernetesDeploy(kubeconfigId: 'kubeconfig',
                        configs: 'serviceA/kube-manifests/deployment.yml',
                        enableConfigSubstitution: true
			)
		}
        }
    }
}

