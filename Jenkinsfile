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
            	git branch: 'main',  url: 'https://github.com/jdvalera/hello-world-war'          	 
        	}    
    	}
    	stage('Build') {
        	steps {
        	sh "mvn clean package"  	 
        	}
    	}
        stage('Ansible Deploy') {
            steps {
                sh "ansible-playbook tomcat_deploy.yaml -i inventory"
            }
        }
    }
}