pipeline {
    agent any
    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"
                git branch: 'main', url: 'https://github.com/kkarthikp/django-notes-app.git'
            }
        }
        stage("Build") {
            steps {
                echo "Building the image"
                sh 'docker build -t notes-app .'
            }
        }
        stage("Push to DockerHub") {
            steps {
                echo "Pushing the image to Dockerhub"
                withCredentials([string(credentialsId: 'docker-hub-credentials', variable: 'DOCKER_HUB_TOKEN')]) {
                    sh "echo $DOCKER_HUB_TOKEN | docker login -u dockarth23 --password-stdin"
                    sh "docker tag notes-app:latest dockarth23/notes-app:latest"
                    sh "docker push dockarth23/notes-app:latest"

                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying in the container"
                //sh "docker run -d -p 8000:8000 dockarth23/notes-app:latest"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
