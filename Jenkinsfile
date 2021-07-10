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
    post {
        always {
            archiveArtifacts artifacts: '**/*.war*', onlyIfSuccessful: true
        }
        success {
            deploy adapters: [tomcat9(url: 'http://35.236.48.65:8080/', 
                                credentialsId: 'tomcat')], 
                            war: 'target/*.war',
                            contextPath: 'app'
            publishHTML target: [
              allowMissing: false,
              alwaysLinkToLastBuild: false,
              keepAll: true,
              reportDir: 'target/site',
              reportFiles: 'surefire-report.html',
              reportName: 'Test Coverage'
            ]
        }
        failure {
            mail to: 'devops.valera@gmail.com',
             subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             body: "Something is wrong with ${env.BUILD_URL}"
        }
    }
}