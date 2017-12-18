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

		
		
		
        
		     
		stage ('Update Build Info and Push change') {

			dir ('wrapper') {
			
			 /*
			 git branch: "master", url: 'https://github.com/emichaf/eiffel-intelligence-artifact-wrapper.git'
			 
                   withCredentials([[$class: 'UsernamePasswordMultiBinding',
                                    credentialsId: 'fbb60332-6a43-489a-87f7-4cea380ad6ca',
                                    usernameVariable: 'GITHUB_USER',
                                    passwordVariable: 'GITHUB_PASSWORD']]) {

                            sh "echo commit = $GIT_LONG_COMMIT >> build_info.txt"


                            sh('git config user.email ${GITHUB_USER}')
                            sh('git config user.name ${GITHUB_USER}')


                            sh('git add .')
                            sh('git commit -m "build info updated"')

                            /*check new commit*/
                            String my_commit = sh(returnStdout: true, script: "git log --format='%H' -n 1").trim()

                            sh("echo my_commit = ${my_commit}")

                            sh("git push http://${GITHUB_USER}:${GITHUB_PASSWORD}@github.com/emichaf/eiffel-intelligence-artifact-wrapper.git")

                   }
				   */
            }

        }
		
		
		


     
	   
 }

}