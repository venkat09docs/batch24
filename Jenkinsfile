def mvnHome
pipeline {
    
	agent none
	
	stages {
		//def mvnHome
		stage ('Preparation') {
		    agent {
		        label 'buildserver'
		    }
		    steps {
			    git 'https://github.com/venkat09docs/batch24'
			    stash 'Source'
			    //script{
			        mvnHome = tool 'maven3.6'
			    //}
		    }
		}    	
	}	
}
