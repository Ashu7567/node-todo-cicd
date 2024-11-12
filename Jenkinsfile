pipeline {
    agent { label "my-slave" }
    stages {
        stage("Code Clone") {
            steps {
                echo "code clone stage"
                git url: "https://github.com/Ashu7567/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build & Test") {
            steps {
                echo "code build stage"
                sh "whoami"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push To Dockerhub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerhubcreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                    sh "docker image tag notes-app:latest ${dockerHubUser}/notes-app:latest"
                    sh "docker push ${dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d --build"
            }
        }
    }
}
