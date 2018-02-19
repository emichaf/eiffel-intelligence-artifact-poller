
node{

     // Poll every minute
     properties([pipelineTriggers([pollSCM('* * * * *')])])

     String GIT_SHORT_COMMIT
     String GIT_LONG_COMMIT
     String WRAPPER_REPO = "https://github.com/emichaf/eiffel-intelligence-artifact-wrapper.git"
     String WRAPPER_REPO_PATH = "github.com/emichaf/eiffel-intelligence-artifact-wrapper.git"
     String WRAPPER_PIPELINE = "eiffel-intelligence-artifact-wrapper"
     String WRAPPER_BRANCH = "master"
     String SOURCE_CODE_REPO = "https://github.com/emichaf/eiffel-intelligence.git"
     String build_info_file = 'build_info.yml'




        stage ('GITHUB Checkout EI BackEnd Artifact SC') {

		    dir ('sourcecode') {


                            git poll: true, branch: "master", url: "$SOURCE_CODE_REPO"

							GIT_SHORT_COMMIT = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()

                            GIT_LONG_COMMIT =  sh(returnStdout: true, script: "git log --format='%H' -n 1").trim()
            }


        }


/*
		stage ('GERRIT - Update Wrapper Build Info and Push Change') {

			dir ('wrapper') {


			// OBS do NOT use git poll: false... it will not work, the build will be triggered circular after the
			// build info file been updated and pushed

			checkout scm: [$class: 'GitSCM',
				  userRemoteConfigs: [[url: "$WRAPPER_REPO"]],
				  branches: [[name: '*/master']]], changelog: false, poll: false


                   withCredentials([[$class: 'UsernamePasswordMultiBinding',
                                    credentialsId: 'fbb60332-6a43-489a-87f7-4cea380ad6ca',
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
*/




docker.image('emtrout/nind23').inside("--privileged"){

	stage ('MyEventTest') {

		    // build job: "${WRAPPER_PIPELINE}/${WRAPPER_BRANCH}", parameters: [[$class: 'StringParameterValue', name: 'param1', value: 'test_param']]


            def json = """{
                            "meta.tags[0]":"my_meta[0]tags",
                            "meta.tags[1]":"my_meta[1]tags",
                            "meta.source.domainId":"my_meta.source.domainId",
                            "meta.source.host":"my_meta.source.host",
                            "meta.source.name":"my_meta.source.name",
                            "meta.source.uri":"my_meta.source.uri",
                            "meta.security.sdm.authorIdentity":"my_meta.security.sdm.authorIdentity",
                            "meta.security.sdm.encryptedDigest":"my_meta.security.sdm.encryptedDigest",
                            "data.submitter.name":"my_data.submitter.name",
                            "data.submitter.email": "${sh(returnStdout: true, script: "git --no-pager show -s --format='%ce' -n 1").trim()}",
                            "data.submitter.id":"my_data.submitter.id",
                            "data.submitter.group":"my_data.submitter.group",
                            "data.gitIdentifier.commitId":"${GIT_LONG_COMMIT}",
                            "data.gitIdentifier.repoUri":"my_data.gitIdentifier.repoUri",
                            "data.gitIdentifier.branch":"my_data.gitIdentifier.branch",
                            "data.gitIdentifier.repoName":"my_data.gitIdentifier.repoName",
                            "data.customData[0]": {"key" : "my.data.customData[0]key", "value" : "my.data.customData[0]value"},
                            "data.customData[1]": {"key" : "my.data.customData[1]key", "value" : "my.data.customData[1]value"},
                            "links[0]": {"type" : "CHANGE", "target" : "e269b37d-17a1-4a10-aafb-c108735ee51f"},
                            "links[1]": {"type" : "PREVIOUS_VERSION", "target" : "e269b37d-17a1-4a10-aafb-c108735ee51a"},
                            "links[2]": {"type" : "PREVIOUS_VERSION", "target" : "e269b37d-17a1-4a10-aafb-c108735ee51b"},
                            "links[3]": {"type" : "CAUSE", "target" : "e269b37d-17a1-4a10-aafb-c108735ee51c"},
                            "links[4]": {"type" : "CAUSE", "target" : "e269b37d-17a1-4a10-aafb-c108735ee50a"},
                            "links[5]": {"type" : "FLOW_CONTEXT", "target" : "e269b37d-17a1-4a10-aafb-c108735ee48a"},
                            "data.svnIdentifier":"<%DELETE%>",
                            "data.ccCompositeIdentifier":"<%DELETE%>",
                            "data.hgIdentifier":"<%DELETE%>"
                          }"""

               // Create Event and publish
               sh "curl -H 'Content-Type: application/json' -X POST --data-binary '${json}' http://docker104-eiffel999.lmera.ericsson.se:9900/doit/?msgType=EiffelSourceChangeSubmittedEvent"



        }

}



 }

