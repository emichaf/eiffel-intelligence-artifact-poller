node{

	stage ('Trigger EI BackEnd SC Component Jobs') {

		    // build job: "${WRAPPER_PIPELINE}/${WRAPPER_BRANCH}", parameters: [[$class: 'StringParameterValue', name: 'param1', value: 'test_param']]

		    // Send event here

            //sh "curl -X GET -u ${EIFFEL_NEXUS_USER}:${EIFFEL_NEXUS_PASSWORD} ${ARM_ARTIFACT_PATH} -o src/main/docker/app.jar"

           // sh "curl -H \"Content-Type: application/json\" -X POST -d '{\"meta.tags[0]\":\"frickenwashere\"}' http://docker104-eiffel999.lmera.ericsson.se:9900/doit/?msgType=EiffelActivityFinishedEvent"

            def json='{"meta.tags[0]":"frickenwashere"}'

            def response = ["curl", "-k", "-X", "POST", "-H", "Content-Type: application/json", "-d", "${json}", "http://docker104-eiffel999.lmera.ericsson.se:9900/doit/?msgType=EiffelActivityFinishedEvent"].execute().text

            println response



        }


}