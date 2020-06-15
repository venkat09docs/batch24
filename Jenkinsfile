def mvnHome
def remote = [:]
  remote.name = 'staging'
  remote.host = '172.31.20.73'
  remote.user = 'centos'
  remote.password = 'Rnstech@123'
  remote.allowAnyHosts = true
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
		stage ('Packaging') {
		    agent {
		        label 'buildserver'
		    }
		    steps {
			   sh "'${mvnHome}/bin/mvn' clean package"	
		    }
		    post{
			success{
			   archiveArtifacts '**/*.war'
			}
		    }
		}  
		stage ('Deploy-Staging') {
		    agent {
		        label 'buildserver'
		    }
		    steps {
			   sh "mv target/*.war target/webapp.war"
                           //sshCommand remote: remote, command: 'sh /home/centos/tomcta8/bin/shutdown.sh'
                           sshPut remote: remote, from: 'target/webapp.war', into: '/opt/tomcat8/webapps/'
                           //sshCommand remote: remote, command: 'sh /home/centos/tomcta8/bin/startup.sh'	
		    }
		    
		} 
		stage ('approve') {
			agent {
				label "buildserver"
            }
			steps {
				timeout(time: 7, unit: 'DAYS') {
					input message: 'Do you want to deploy?', submitter: 'admin'
				}
			}
		}
		stage ('Prod-Deployment') {
		    agent {
		        label 'buildserver'
		    }
		    steps {
			   sh "'${mvnHome}/bin/mvn' clean package"	
		    }
		    post{
			success{
			   archiveArtifacts '**/*.war'
			}
		    }
		} 
	}	
}
