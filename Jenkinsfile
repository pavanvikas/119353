pipeline {

    agent any

    environment {


        DOCKER_REPO= "docker-local"

        DEBIAN_REPO = "debian-local"
        WORKSPACE ="/tmp/119353"

    }

    stages {
/* stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "Arti1",
                    url: "http://myartifactory.com/artifactory"
                )    
            }
        } */
         stage("Pull  Debian") {

            steps {

                    dir("${WORKSPACE}") {

                    sh "wget http://ppa.launchpad.net/deadsnakes/ppa/ubuntu/pool/main/p/python3.7/libpython3.7-dev_3.7.5-1+xenial1_amd64.deb"

                }

            }

        }
        stage("Publish debain to artifactory") {

            steps {

                script {

                    rtBuildInfo (

                        maxBuilds: 3,

                        doNotDiscardBuilds: ["3"],

                        captureEnv: true,

                        deleteBuildArtifacts: true,

                        buildName : "${JOB_NAME}",

                        buildNumber:  "${BUILD_NUMBER}"

                    )

                    rtUpload (

                        serverId: "Arti1",

                        buildName: "${JOB_NAME}",

                        buildNumber: "${BUILD_NUMBER}",

                        spec: """{

                                "files": [

                                    {

                                        "pattern": "${WORKSPACE}",

                                        "target": "${DEBIAN_REPO}/",

                                        "props" : "deb.distribution=xenial;deb.component=main;deb.architecture=amd64"

                                    }

                                ]

                            }"""

                    )

                    rtPublishBuildInfo (

                        serverId: "Arti1",

                        buildName: "${JOB_NAME}",

                        buildNumber: "${BUILD_NUMBER}"

                    )

                }

            }

        }
        
        
        
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
                script{
                    def awsbuildInfo = newBuildInfo()
                     awsbuildInfo.setName("docker-${JOB_NAME}")
                    def server = Artifactory.server "Arti1"
                    def rtDocker = Artifactory.docker server: server
                    awsbuildInfo.retention(["maxBuilds": 2,  "deleteBuildArtifacts" : true])
                    collectEnv(awsbuildInfo.getEnv())
                  // def buildInfo =  awsbuildInfo.append(rtDocker.push("artifactory.jfrog.com/test-docker/alpine:latest", "${DOCKER_REPO}"))
                 // def buildInfo1 =   awsbuildInfo.append(rtDocker.push("artifactory.jfrog.com/test-docker/golang:latest", "${DOCKER_REPO}"))                 
                  def buildInfo =  rtDocker.push("artifactory.jfrog.com/test-docker/alpine:latest", "${DOCKER_REPO}")
                def buildInfo1 =  rtDocker.push("artifactory.jfrog.com/test-docker/golang:latest", "${DOCKER_REPO}")

                    
                    server.publishBuildInfo buildInfo
                 server.publishBuildInfo buildInfo1

              //  rtDockerPush(
              //     serverId: "Arti1",
              //    image: "docker.myartifactory.com/test-docker/alpine:latest",
              //    targetRepo: 'docker-local',
              //    buildName: "119353",
              //    buildNumber: "21",
              //  )   
                    
            }
            }

        }


    }
}
