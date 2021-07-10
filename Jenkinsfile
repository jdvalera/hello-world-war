pipeline {
	agent any
	tools {
    	maven 'local-mvn'
	}
    triggers {
        pollSCM '0 0 * * *'
    }
	stages {
    	stage("Checkout") {   
        	steps {               	 
            	git branch: 'master',  url: 'https://github.com/jdvalera/hello-world-war'          	 
        	}    
    	}
    	stage('Build') {
        	steps {
        	sh "mvn clean package"  	 
        	}
    	}
        stage('Ansible Deploy') {
            steps {
                ansiblePlaybook( 
                    playbook: 'tomcat_deploy.yaml',
                    inventory: 'inventory.ini', 
                    credentialsId: '8beb1c00-0659-4013-ba79-f32be5238ce0') 
            }
        }
    }
}