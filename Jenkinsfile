node{


docker.image('emtrout/nind23').inside("--privileged"){

	stage ('MyEventTest') {


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
                            "data.submitter.email":"my_data.submitter.email",
                            "data.submitter.id":"my_data.submitter.id",
                            "data.submitter.group":"my_data.submitter.group",
                            "data.gitIdentifier.commitId":"my_data.gitIdentifier.commitId",
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