pipeline {
    agent any
environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhubcredintial')
	}
    stages {
        stage('Clone repository') {
            steps {
                sh 'rm -rf *'
               sh "https://github.com/sadafansaribgmi/Docker1.git"
               
                //sh "cd cicd-projects"
               
            }
        }

        stage('Build Docker image') {
            
            steps {
                sh 'ls -la'
                sh 'docker build -t sadaf46/php:v${BUILD_NUMBER} -f Docker1/first-cicd-jenkins-docker-route53/Dockerfile .'
            }
        }

        stage('Login & Push Docker image') {
            
            steps {
 				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'

                sh 'docker push makmat/php:v${BUILD_NUMBER}'
            }
        }
        stage('Run Docker container') {
            steps {
                sh '''
containers=$(docker ps --format "{{.ID}}:{{.Ports}}" | grep ":80->" | awk -F ':' '{print $1}')
if [ -z "$containers" ]; then
    echo "No containers using port 80 found."
else
    echo "Containers using port 80:"
    echo "$containers"

    # Delete containers using port 80
    for container in $containers; do
        echo "Deleting container with ID: $container"
        docker rm -f $container
    done
fi
		'''
                sh 'docker run -d -p 80:80 sadaf46/php:v${BUILD_NUMBER}'
            }
        }

        stage('Show success message') {
            steps {
                echo 'Docker container is running in our container'
            }
        }
    }
}
