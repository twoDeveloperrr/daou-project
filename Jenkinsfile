pipeline {
    agent any

    stages {   
        stage('Git Clone') {
            steps {  
                script { 
                    try{
                        git url: "https://github.com/twoDeveloperrr/source-nodejs.git", branch: "master" 
                        env.cloneResult=true
                    }catch(error){
                        print(error)
                        env.cloneResult=false
                         currentBuild.result='FAILURE'
                    }
                }
          }
        }

	stage('SSH transfer') {
	    steps([$class: 'BapSshPromotionPublisherPlugin]){
		sshPublisher(
		    continueOnError: false, failOnError: true,
		    publishers: [
			sshPublisherDesc(
			    configName: "",
			    verbose: true,
			    transfers: [
				sshTransfer(
				    sourceFiles: "playbook/prometheus-install-nodeporter.yaml",
				    removePrefix: "playbook",
				    remoteDirectory: "",
				    execCommand: "ansible-playbook prometheus-install-nodeporter.yaml"
				)
			    ]
			)
		    ]
		)
	    }
	}
  }
}
