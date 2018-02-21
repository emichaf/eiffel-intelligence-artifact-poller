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

     String COMITTER_NAME
     String COMITTER_MAIL
     String COMITTER_DATE
     String COMITTER_ID
     String AUTHOR_NAME
     String AUTHOR_MAIL
     String AUTHOR_DATE
     String AUTHOR_ID
     String GIT_FILES
     String GIT_INSERTED
     String GIT_DELETED


        stage ('GITHUB Checkout EI BackEnd Artifact SC') {

		    dir ('sourcecode') {


                            git poll: true, branch: "master", url: "$SOURCE_CODE_REPO"

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


                           // GIT_LONG_COMMIT =  sh(returnStdout: true, script: "git log --format='%H' -n 1").trim()
                           // String hej = sh(returnStdout: true, script: "git log --shortstat -n 1 | (grep 'file changed' || grep 'files changed') | awk '{$GIT_FILES+=$1; $GIT_INSERTED+=$4; $GIT_DELETED+=$6} END'")

                         //String test =  sh(returnStdout: true, script: "git log --shortstat -n 1 | (grep 'file changed' || grep 'files changed') | awk '{files+=$1;} END {print files'").trim()

                        sh("git log --shortstat -n 1 | (grep 'file changed' || grep 'files changed') | awk '{files+=$1;} END {print files}'")

                        sh("awk -W version")

                         // | (grep 'file changed' || grep 'files changed') | awk '{files+=$1;} END {print files}'")




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




	stage ('Create Event') {

		    // build job: "${WRAPPER_PIPELINE}/${WRAPPER_BRANCH}", parameters: [[$class: 'StringParameterValue', name: 'param1', value: 'test_param']]



            // EiffelSourceChangeCreatedEvent
            def json_scc = """{
                            "meta.tags[0]":"my_meta[0]tags",
                            "meta.tags[1]":"my_meta[1]tags",
                            "meta.source.domainId":"my_meta.source.domainId",
                            "meta.source.host":"docker104-eiffel999",
                            "meta.source.name":"my_meta.source.name",
                            "meta.source.uri":"my_meta.source.uri",
                            "meta.security.sdm.authorIdentity":"my_meta.security.sdm.authorIdentity",
                            "meta.security.sdm.encryptedDigest":"my_meta.security.sdm.encryptedDigest",
                            "data.author.name":"${AUTHOR_NAME}",
                            "data.author.email":"${AUTHOR_MAIL}",
                            "data.author.id":"${AUTHOR_ID}",
                            "data.author.group":"",
                            "data.change.insertions":"${GIT_INSERTED}",
                            "data.change.deletions":"${GIT_DELETED}",
                            "data.change.files":"${GIT_FILES}",
                            "data.change.tracker":"my_data.change.tracker",
                            "data.change.details":"my_data.change.details",
                            "data.change.id":"my_data.change.id",
                            "data.issues[0].type":"BUG",
                            "data.issues[0].tracker":"my_data.issues.tracker",
                            "data.issues[0].id":"my_data.issues.id",
                            "data.issues[0].uri":"my_data.issues.uri",
                            "data.issues[0].transition":"RESOLVED",
                            "data.gitIdentifier.commitId":"${GIT_LONG_COMMIT}",
                            "data.gitIdentifier.repoUri":"${SOURCE_CODE_REPO}",
                            "data.gitIdentifier.branch":"master",
                            "data.gitIdentifier.repoName":"eiffel-intelligence.git",
                            "data.customData[0]": {"key" : "my.data.customData[0]key", "value" : "my.data.customData[0]value"},
                            "data.customData[1]": {"key" : "my.data.customData[1]key", "value" : "my.data.customData[1]value"},
                            "links[0]": {"type" : "BASE", "target" : "e269b37d-17a1-4a10-aafb-c108735ee51f"},
                            "links[1]": {"type" : "PREVIOUS_VERSION", "target" : "e269b37d-17a1-4a10-aafb-c108735ee51a"},
                            "links[2]": {"type" : "PREVIOUS_VERSION", "target" : "e269b37d-17a1-4a10-aafb-c108735ee51b"},
                            "links[3]": {"type" : "CAUSE", "target" : "e269b37d-17a1-4a10-aafb-c108735ee51c"},
                            "links[4]": {"type" : "CAUSE", "target" : "e269b37d-17a1-4a10-aafb-c108735ee50a"},
                            "links[5]": {"type" : "FLOW_CONTEXT", "target" : "e269b37d-17a1-4a10-aafb-c108735ee48a"},
                            "data.svnIdentifier":"<%DELETE%>",
                            "data.ccCompositeIdentifier":"<%DELETE%>",
                            "data.hgIdentifier":"<%DELETE%>"
                          }"""

            // Create SCC Event and publish
            sh "curl -H 'Content-Type: application/json' -X POST --data-binary '${json_scc}' http://docker104-eiffel999.lmera.ericsson.se:9900/doit/?msgType=EiffelSourceChangeCreatedEvent"



            // EiffelSourceChangeSubmittedEvent
            def json_scs = """{
                            "meta.tags[0]":"my_meta[0]tags",
                            "meta.tags[1]":"my_meta[1]tags",
                            "meta.source.domainId":"docker104-eiffel999",
                            "meta.source.host":"my_meta.source.host",
                            "meta.source.name":"my_meta.source.name",
                            "meta.source.uri":"http://docker104-eiffel999.lmera.ericsson.se:8080",
                            "meta.security.sdm.authorIdentity":"my_meta.security.sdm.authorIdentity",
                            "meta.security.sdm.encryptedDigest":"my_meta.security.sdm.encryptedDigest",
                            "data.submitter.name":"${COMITTER_NAME}",
                            "data.submitter.email": "${COMITTER_MAIL}",
                            "data.submitter.id":"${COMITTER_ID}",
                            "data.submitter.group":"",
                            "data.gitIdentifier.commitId":"${GIT_LONG_COMMIT}",
                            "data.gitIdentifier.repoUri":"${SOURCE_CODE_REPO}",
                            "data.gitIdentifier.branch":"master",
                            "data.gitIdentifier.repoName":"eiffel-intelligence.git",
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

               // Create SCS Event and publish
               sh "curl -H 'Content-Type: application/json' -X POST --data-binary '${json_scs}' http://docker104-eiffel999.lmera.ericsson.se:9900/doit/?msgType=EiffelSourceChangeSubmittedEvent"



        }





 }

