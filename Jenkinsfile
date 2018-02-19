node{

	stage ('mintest') {


            json='{"meta.tags[0]":"frickenwashere"}'

            def response = ["curl", "-k", "-X", "POST", "-H", "Content-Type: application/json", "-d", "${json}", "http://docker104-eiffel999.lmera.ericsson.se:9900/doit/?msgType=EiffelActivityFinishedEvent"].execute().text

            //println response

            println "hej"


        }


}