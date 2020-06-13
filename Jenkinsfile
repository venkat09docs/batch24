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
			    script{
			        mvnHome = tool 'maven3.6'
			    }
		    }
		}    
		stage ('Static-Analysis') {
		    agent {
		        label 'buildserver'
		    }
		    steps {
			   sh "'${mvnHome}/bin/mvn' clean cobertura:cobertura"	
		    }
		    post{
			success{
			   cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
			}
		    }
		}   
		stage ('Pre-Deployment-Testing') {
		    agent {
		        label 'buildserver'
		    }
		    steps {
			   sh "'${mvnHome}/bin/mvn' clean test"	
		    }
		    post{
			success{
			   junit 'target/surefire-reports/*.xml'
			}
		    }
		}  
	}	
}
