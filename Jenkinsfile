node{


     properties([pipelineTriggers([pollSCM('* * * * *')])])

     String GIT_SHORT_COMMIT
     String GIT_LONG_COMMIT
  //test

 docker.withServer('tcp://docker104-eiffel999.lmera.ericsson.se:4243', 'remote_docker_host') {

  
 
 

        stage ('GITHUB Checkout EI Artifact SC') {
	    
		    dir ('sourcecode') {
	                        
									   
                            git branch: "master", url: 'https://github.com/emichaf/eiffel-intelligence.git'                          
                           
							GIT_SHORT_COMMIT = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()

                            GIT_LONG_COMMIT =  sh(returnStdout: true, script: "git log --format='%H' -n 1").trim()
            }
              
			  
        }

		
		
		
      


     
	   
 }

}