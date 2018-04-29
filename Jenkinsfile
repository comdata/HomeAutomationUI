pipeline {
    agent {
        docker {
            image 'maven:3.5.2-jdk-8-alpine' 
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
					sh 'mvn -T 1C -N install'
			}
		}
		
	
	
	    stage('Deploy') {
	       parallel {
	      	
	   			stage('Deploy Frontend') {
	        		steps {
	       				sh 'rsync -auv WebContent/* root@192.168.1.76:/var/lib/tomcat8/webapps/HomeAutomationUI'
	   				}
	   			}
	   		}	
	    }
	    //stage('Restart') {
	    //    steps {
	    //   		sh 'ssh root@192.168.1.76 /etc/init.d/tomcat8 restart'
	   	//	}
	    //}
    }
}
