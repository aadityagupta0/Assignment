pipeline {
    agent any
	tools
	{
		maven "Apache Maven 3.3.9"
		 jdk "JAVA"
	}
	
    stages {
        stage('CheckOut')
        {
            steps
            {
                checkout scm
            }
        }
        stage('Build') 
        {
            steps 
            {
                echo 'Addition Program'
		            echo 'Building the program'
                bat  "mvn package"
            }
        }
	
        stage('Unit Test')
        {
            steps
            {
		            echo 'Testing Stage'
                bat 'mvn test'
            }
        }
        stage('Sonar Analysis') 
        {
            steps 
            {
		            echo 'Performing Sonar Analysis'
                withSonarQubeEnv("Sonar Qube Scanner")
                //withSonarQubeEnv(credentialsId: '09015078aabf8896496179114579b897c6676bc0', installationName: 'sonar scanner') 
                {
                    //bat "mvn sonar:sonar"
			mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar
                }   
            }
        }
        stage('Artifactory')
        {
	        steps
	        {
		        rtMavenDeployer (
    			    id: 'deployer',
		            serverId: 'artifactory',
		            releaseRepo: 'example-repo-local',
		            snapshotRepo: 'example-repo-local' 
		        )
		        rtMavenRun (
		        pom: 'pom.xml',
		        goals: 'clean install',
		        deployerId: 'deployer'
			)
		        rtPublishBuildInfo (
		            serverId: 'artifactory' 
		                )
	        }
	}

        stage('Release') {
            steps 
	    {
                echo 'Releasing'
            }
        }
	stage ('deploy')
	    {
		    steps
		    {
			    bat 'dir'
			    bat 'xcopy /S /Q /Y /F target\\*.war "C:\\Program Files\\Apache Software Foundation\\Tomcat 8.5\\webapps"'
			    //bat 'C:\\Program Files\\Apache Software Foundation\\Tomcat 8.5\\bin\\catalina.bat restart'
		    }
	    }
    }
}
