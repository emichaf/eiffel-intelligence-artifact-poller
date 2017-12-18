node{

     String GIT_SHORT_COMMIT
     String GIT_LONG_COMMIT


 docker.withServer('tcp://docker104-eiffel999.lmera.ericsson.se:4243', 'remote_docker_host') {

   /*
     For inside() to work, the Docker server and the Jenkins agent must use the same filesystem,
     so that the workspace can be mounted.

     When Jenkins detects that the agent is itself running inside a Docker container, it will automatically pass
     the --volumes-from argument to the inside container, ensuring that it can share a workspace with the agent.


     triggers {
                pollSCM 'H/1 * * * *'
            }


            sh "git rev-parse --short HEAD > .git/commit-id"
            commit_id = readFile('.git/commit-id')

     */

       stage ('GIT Checkout') {
                            git branch: "master", url: 'https://github.com/emichaf/eiffel-intelligence-artifact-wrapper.git'

                            GIT_SHORT_COMMIT = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()

                            GIT_LONG_COMMIT =  sh(returnStdout: true, script: "git log --format='%H' -n 1").trim()


        }


        /* här ska commit hämtas för stämplingen i build_info filen i innersource wrapper repot -> Gerrit */
        stage ('GITHUB Checkout EI SC') {

                dir ('sourcecode') {




                                git branch: "master", url: 'https://github.com/Ericsson/eiffel-intelligence.git'


                            }
            }




        docker.image('emtrout/dind:latest').inside {
             stage('UnitTests & FlowTests)') {

                            withCredentials([[$class: 'UsernamePasswordMultiBinding',
                            credentialsId: 'e7de4146-4a59-4406-916e-d10506cfaeb8',
                            usernameVariable: 'DOCKER_HUB_USER',
                            passwordVariable: 'DOCKER_HUB_PASSWORD']]) {

                                // OBS privileged: true for image for embedded mongodb (flapdoodle) to work

							    dir ('sourcecode') {

									 def travis_datas = readYaml file: ".travis.yml"

                                     // Execute tests in travis file
									 travis_datas.script.each { item ->

									    sh "$item"

									 };


									 sh "ls"

							    }


							}

			}
        }





        stage ('Update Build Info and Push change') {

                   withCredentials([[$class: 'UsernamePasswordMultiBinding',
                                    credentialsId: 'fbb60332-6a43-489a-87f7-4cea380ad6ca',
                                    usernameVariable: 'GITHUB_USER',
                                    passwordVariable: 'GITHUB_PASSWORD']]) {

                            sh "echo commit = $GIT_LONG_COMMIT >> build_info.txt"


                            //sh('git config user.email ${GITHUB_USER}')
                            //sh('git config user.name ${GITHUB_USER}')


                            sh('git add .')
                            sh('git commit -m "build info updated"')

                            /*check new commit*/
                            String my_commit = sh(returnStdout: true, script: "git log --format='%H' -n 1").trim()

                            sh("echo my_commit = ${my_commit}")

                            sh("git push http://${GITHUB_USER}:${GITHUB_PASSWORD}@github.com/emichaf/eiffel-intelligence-artifact-wrapper.git")

                   }

        }




        docker.image('emtrout/dind:latest').inside {


               stage('Maven Build') {

                             sh "mvn clean package -DskipTests"

                        }




               stage ('Test') {

                              parallel (
                                'Test Server' : {
                                  sh 'ls'
                                },
                                'Test Sample Client' : {
                                  sh 'ls'
                                }
                              )
                            }



               stage('Build and Push Docker Image') {

                        withCredentials([[$class: 'UsernamePasswordMultiBinding',
                                    credentialsId: '7b05ac28-c1ae-4249-a0c6-7c54c74e3b67',
                                    usernameVariable: 'DOCKER_HUB_USER',
                                    passwordVariable: 'DOCKER_HUB_PASSWORD']]) {


                           pom = readMavenPom file: 'pom.xml'

                           sh "docker login -u ${env.DOCKER_HUB_USER} -p ${env.DOCKER_HUB_PASSWORD}"

                           sh "docker build --no-cache=true -t ${env.DOCKER_HUB_USER}/${pom.artifactId}:latest -f src/main/docker/Dockerfile src/main/docker/"

                           sh "docker push ${env.DOCKER_HUB_USER}/${pom.artifactId}:latest"

                           sh "docker build --no-cache=true -t ${env.DOCKER_HUB_USER}/${pom.artifactId}:${GIT_SHORT_COMMIT} -f src/main/docker/Dockerfile src/main/docker/"

                           sh "docker push ${env.DOCKER_HUB_USER}/${pom.artifactId}:${GIT_SHORT_COMMIT}"

                           sh "docker logout"

                        }
                    }


               stage('Deploy to K8S Stage') {
               /*
                                   container('kubectl') {
                                      withCredentials([[
                                       $class: 'FileBinding',
                                       credentialsId: '77bd756d-382a-4704-bb9c-9d52f023ac4d',
                                       variable: 'KUBECONFIG'
                                   ]]){


                                        sh("kubectl --kubeconfig=$KUBECONFIG create -f ./target/classes/META-INF/fabric8/kubernetes.yml")
                                        sh("kubectl --kubeconfig=$KUBECONFIG get pods")

                                      }
                                   }
                                   */
               }



               stage ('Integration Test') {

                                     parallel (
                                       'Test Server' : {
                                         sh 'ls'
                                       },
                                       'Test Sample Client' : {
                                         sh 'ls'
                                       }
                                     )

               }



        } /*docker.image('emtrout/dind:latest').inside {*/

 }

}
