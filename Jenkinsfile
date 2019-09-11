pipeline {
    agent {
        docker {
            image 'maven:3.5.4-jdk-8-alpine'
		//-v /var/jenkins_home/.m2:/root/.m2 
            args '-v /root/.ssh:/root/.ssh' 
        }
    }
    stages {
		stage('Prepare') {
		    steps {
			sh 'apk update'
			sh 'apk add rsync openssh openrc git'
			} 
		}
		stage('Build') { 
			steps {
				withMaven() {
					sh 'MVN_CMD -T 1C -N install'
				}
			}
		}
	    stage('Deploy') {
	       parallel {
	   			stage('Deploy Frontend') {
	        		steps {
	       				sh 'rsync -auv WebContent/* root@192.168.1.76:/var/lib/tomcat8/webapps/HomeAutomationUI'
	   				}
	   			}
	   			//stage('Sonarqube') {
		   		//	steps {
		   				//org.jacoco:jacoco-maven-plugin:prepare-agent
		   		    	//sh 'MVN_CMD -DskipTests=true  sonar:sonar -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=$SONAR_TOKEN -Dsonar.organization=homeautomation'
		   		//	}
		   		//}	
	   		}	
	    }
    }
}
