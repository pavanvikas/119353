pipeline {

    agent any

    environment {


        DOCKER_REPO= "docker-local"

        DEBIAN_REPO = "debian-local"
        WORKSPACE ="/tmp/119353"

    }

    stages {

      
        stage("Pull docker images") {

            steps {

                script {

                    sh "docker pull alpine:latest"

                    sh "docker pull golang:latest"

                    sh "docker tag alpine:latest docker.myartifactory.com/test-docker/alpine:latest"

                    sh "docker tag golang:latest docker.myartifactory.com/test-docker/golang:latest"

                }

            }

        }

        stage("Push docker images") {

            steps {

                rtDockerPush(
                  serverId: 'Arti1',
                  image: "docker.myartifactory.com/test-docker/alpine:latest",
                  targetRepo: 'docker-local/'
                )           
            }

        }

stage ('Publish build info') {
    steps {
                rtPublishBuildInfo (
                    buildName: 'MK',
                    buildNumber: '48',
                    serverId: "Arti1"
                )
            }
        }
    }

}
