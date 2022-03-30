pipeline {
    agent any

    stages {   
        stage('Git Clone') {
            steps {  
                script { 
                    try{
                        git url: "https://github.com/twoDeveloperrr/daou-project.git", branch: "master" 
                        env.cloneResult=true
                    }catch(error){
                        print(error)
                        env.cloneResult=false
                         currentBuild.result='FAILURE'
                    }
                }
          }
        }

	stage('SSH transfer ansible-host prometheus-host') {
	    steps([$class: 'BapSshPromotionPublisherPlugin']){
		sshPublisher(
		    continueOnError: false, failOnError: true,
		    publishers: [
			sshPublisherDesc(
			    configName: "ansible-host",
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
		sshPublisher(
		    continueOnError: false, failOnError: true,
		    publishers: [
			sshPublisherDesc(
			    configName: "prometheus-host",
			    verbose: true,
			    transfers: [
				sshTransfer(
				    sourceFiles: "monitoring-server/prometheus/**.yaml",
				    removePrefix: "monitoring-server",
				    remoteDirectory: "test",
				    execCommand: "mkdir test2"
				)
			    ]
			)
		    ]
		)
	    }
	}

  }
}
