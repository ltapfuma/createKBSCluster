pipeline {
	agent any
	stages {

		stage('Linting') {
			steps {
				sh 'tidy -q -e *.html'
			}
		}
		
		stage('Build Image') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
                                                docker build -t ltapfuma/capstone-udacity .
					'''
				}
			}
		}

		stage('Push Image To Dockerhub') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                                                docker push ltapfuma/capstone-udacity
					'''
				}
			}
		}

		stage('Set Current kubectl Context') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_credentials') {
					sh '''
						kubectl config use-context arn:aws:eks:us-east-1:835432431333:cluster/deploy1
					'''
				}
			}
		}

		stage('Deploy Blue Container') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_credentials') {
					sh '''
						kubectl apply -f blue_deployment.yml
					'''
				}
			}
		}

		stage('Deploy green container') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_credentials') {
					sh '''
						kubectl apply -f green_deployment.yml
					'''
				}
			}
		}

		stage('Create Service Pointing to Blue Replication Controller') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_credentials') {
					sh '''
						kubectl apply -f blue_service.yml
					'''
				}
			}
		}

		stage('Approval for Redirection') {
            steps {
                input "Ready to redirect traffic to green replication controller ?"
            }
        }

		stage('Create Service Pointing to Green Replication Controller') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_credentials') {
					sh '''
						kubectl apply -f green_service.yml
					'''
				}
			}
		}

	}
}

