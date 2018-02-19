import wslite.http.auth.*
import wslite.rest.*
node{

	stage ('mintest') {


            //json='{"meta.tags[0]":"frickenwashere"}'

            //def response = ["curl", "-k", "-X", "POST", "-H", "Content-Type: application/json", "-d", "${json}", "http://docker104-eiffel999.lmera.ericsson.se:9900/doit/?msgType=EiffelActivityFinishedEvent"].execute().text

            //println response

           // println "${json}"


      script{
            def json = """{
                    "meta.tags[0]":"my_meta[0]tags",
                     "meta.tags[1]":"my_meta[1]tags",
                     "meta.source.domainId":"my_meta.source.domainId",
                     "meta.source.host":"my_meta.source.host",
                     "meta.source.name":"my_meta.source.name",
                     "meta.source.uri":"my_meta.source.uri",
                     "meta.security.sdm.authorIdentity":"my_meta.security.sdm.authorIdentity",
                     "meta.security.sdm.encryptedDigest":"my_meta.security.sdm.encryptedDigest",
                     "data.outcome.conclusion":"SUCCESSFUL",
                     "data.outcome.description":"my_data.outcome.description",
                     "data.customData[0]": {"key" : "my.data.customData[0]key", "value" : "my.data.customData[0]value"},
                     "data.customData[1]": {"key" : "my.data.customData[1]key", "value" : "my.data.customData[1]value"},
                     "data.persistentLogs[0]": {"name" : "my_data.persistentLogs[0]name", "uri" : "my_data.persistentLogs[0]uri"},
                     "data.persistentLogs[1]": {"name" : "my_data.persistentLogs[1]name", "uri" : "my_data.persistentLogs[1]uri"},
                     "data.persistentLogs[2]": {"name" : "my_data.persistentLogs[2]name", "uri" : "my_data.persistentLogs[2]uri"},
                     "links[0]": {"type" : "ACTIVITY_EXECUTION", "target" : "e269b37d-17a1-4a10-aafb-c108735ee51f"}
                }"""


               // process = ["curl", "-k", "--user", "user:pass", "-X", "POST", "-H", "Content-Type: application/json", "-d", "${json}", "https://<api_uri>/launch"].execute().text

               response = ["curl", "-k", "-X", "POST", "-H", "Content-Type: application/json", "-d", "${json}", "http://docker104-eiffel999.lmera.ericsson.se:9900/doit/?msgType=EiffelActivityFinishedEvent"].execute().text

                println "${response}"

        }
      }


}