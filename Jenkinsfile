node{

     // Poll every minute
     properties([pipelineTriggers([pollSCM('* * * * *')])])

     // OLD service String EVENT_PARSER_PUB_GEN_URI = 'http://docker104-eiffel999.lmera.ericsson.se:9900/doit/?msgType='
	 String EVENT_PARSER_PUB_GEN_URI = 'http://docker104-eiffel999.lmera.ericsson.se:8096/generateAndPublish/?mp=eiffelsemantics&msgType='

     String GIT_SHORT_COMMIT
     String GIT_LONG_COMMIT

     String WRAPPER_REPO = "https://github.com/emichaf/eiffel-intelligence-artifact-wrapper.git"
     String WRAPPER_REPO_PATH = "github.com/emichaf/eiffel-intelligence-artifact-wrapper.git"
   //String WRAPPER_PIPELINE = "eiffel-intelligence-artifact-wrapper"
     String WRAPPER_BRANCH = "master"

     String SOURCE_CODE_REPO_URI = "https://github.com/emichaf/eiffel-intelligence.git"
     String SOURCE_CODE_REPO_NAME
     String SOURCE_CODE_CHANGE_DETAILS_URI
     String SOURCE_CODE_CHANGE_DETAILS_TRACKER = 'GitHub'
     String SOURCE_CODE_BRANCH = "master"
     String build_info_file = 'build_info.yml'
     String COMITTER_NAME
     String COMITTER_MAIL
     String COMITTER_DATE
     String COMITTER_ID
     String AUTHOR_NAME
     String AUTHOR_MAIL
     String AUTHOR_DATE
     String AUTHOR_ID
     String GIT_FILES_NO
     String GIT_INSERTED
     String GIT_DELETED
     String GIT_COMMIT_MSG
     String HOST_NAME
     String DOMAIN_ID
     String SOURCE_NAME = "Jenkins"

        stage ('GITHUB Checkout EI BackEnd Artifact SC') {

		    dir ('sourcecode') {

                            // OBS ONLY WORKING IF GIT CLONE/PULL BEEN PERFORMED
                            //SOURCE_CODE_REPO_URI = sh(returnStdout: true, script: "git remote get-url origin").trim()

                            git poll: true, branch: "$SOURCE_CODE_BRANCH", url: "$SOURCE_CODE_REPO_URI"

                            SOURCE_CODE_REPO_NAME = sh(returnStdout: true, script: "basename -s .git `git config --get remote.origin.url`").trim()

							GIT_SHORT_COMMIT = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
                            GIT_LONG_COMMIT =  sh(returnStdout: true, script: "git log --format='%H' -n 1").trim()

                            COMITTER_MAIL = sh(returnStdout: true, script: "git --no-pager show -s --format='%ce' -n 1").trim()
                            COMITTER_DATE = sh(returnStdout: true, script: "git --no-pager show -s --format='%ct' -n 1").trim()
                            COMITTER_ID = sh(returnStdout: true, script: "git --no-pager show -s --format='%cn' -n 1").trim()
                            String[] comitter_parts = COMITTER_MAIL.split('@');
                            COMITTER_NAME = comitter_parts[0].replace(".", " ");

                            AUTHOR_MAIL = sh(returnStdout: true, script: "git --no-pager show -s --format='%ae' -n 1").trim()
                            AUTHOR_DATE = sh(returnStdout: true, script: "git --no-pager show -s --format='%at' -n 1").trim()
                            AUTHOR_ID = sh(returnStdout: true, script: "git --no-pager show -s --format='%an' -n 1").trim()
                            String[] author_parts = AUTHOR_MAIL.split('@');
                            AUTHOR_NAME = author_parts[0].replace(".", " ");

                            GIT_COMMIT_MSG = sh(returnStdout: true, script: "git --no-pager show -s --format='%s' -n 1").trim()

                            SOURCE_CODE_CHANGE_DETAILS_URI = "${SOURCE_CODE_REPO_URI.replace(".git", "")}" + "/commit/" + "${GIT_LONG_COMMIT}"

                            GIT_FILES_NO = sh(returnStdout: true, script: "git log --shortstat -n 1 | (grep 'file changed' || grep 'files changed') | awk '{files+=\$1;} END {print files}'").trim()
                            GIT_INSERTED = sh(returnStdout: true, script: "git log --shortstat -n 1 | (grep 'file changed' || grep 'files changed') | awk '{inserted+=\$4;} END {print inserted}'").trim()
                            GIT_DELETED = sh(returnStdout: true, script: "git log --shortstat -n 1 | (grep 'file changed' || grep 'files changed') | awk '{deleted+=\$6;} END {print deleted}'").trim()


                            HOST_NAME = sh(returnStdout: true, script: "hostname").trim()
                            DOMAIN_ID = sh(returnStdout: true, script: " domainname").trim()



                            //String testar = sh(returnStdout: true, script: "git show --pretty='format:' --name-only -n 1 | awk '{\$1}{print \$1\",\"}'").trim()

            }


        }



		stage ('GERRIT - Update Wrapper Build Info and Push Change') {

			dir ('wrapper') {


			// OBS do NOT use git poll: false... it will not work, the build will be triggered circular after the
			// build info file been updated and pushed

			checkout scm: [$class: 'GitSCM',
				  userRemoteConfigs: [[url: "$WRAPPER_REPO"]],
				  branches: [[name: '*/master']]], changelog: false, poll: false


                   withCredentials([[$class: 'UsernamePasswordMultiBinding',
                                    credentialsId: 'GITHUB_CREDENTIALS',
                                    usernameVariable: 'GITHUB_USER',
                                    passwordVariable: 'GITHUB_PASSWORD']]) {




							// Update build_info.yml file with github hash
							def exists = fileExists "$build_info_file"
								if (exists) {
									sh "rm $build_info_file"
								}

							def yml_content = ['commit': "$GIT_LONG_COMMIT"]
                            writeYaml file: "$build_info_file", data: yml_content


                            sh('git config user.email ${GITHUB_USER}')
                            sh('git config user.name ${GITHUB_USER}')


                            sh('git add .')
                            sh('git commit -m "build info file updated"')

                            sh("git push http://${GITHUB_USER}:${GITHUB_PASSWORD}@${WRAPPER_REPO_PATH} HEAD:${WRAPPER_BRANCH}")

                   }

            }

        }




	stage ('Create Event') {

		    // build job: "${WRAPPER_PIPELINE}/${WRAPPER_BRANCH}", parameters: [[$class: 'StringParameterValue', name: 'param1', value: 'test_param']]


            // print existing env vars
            echo sh(returnStdout: true, script: 'env')

            jenkins_display_url = "${RUN_DISPLAY_URL}".replaceAll("unconfigured-jenkins-location","$JENKINS_HOSTNAME"+":"+"${JENKINS_HOSTPORT}")


        }


         // Clean up workspace
         step([$class: 'WsCleanup'])


 }
