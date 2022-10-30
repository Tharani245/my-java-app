pipeline {
    agent any
    environment {
	   tomcatWeb = 'D:/Programfiles/Tomcat/webapps'
           tomcatBin = 'D:/Programfiles/Tomcat/bin'
           tomcatStatus = ''
    }	
    stages {
        stage("git clone") {
            steps {
                git branch: 'master',url: 'https://github.com/Tharani245/my-java-app.git'
            }
        }
        stage("build") {
            steps {
                sh "mvn clean install"
            }
        }
	stage("Test") {
            steps {
                // sh "mvn test"
                // junit allowEmptyResults: true, testResults: '**/test-results/*.xml'
                // junit '**/test-results/*.xml'
		// sh 'ln -s C:\Users\Shanmugam K\.jenkins\workspace\Declarative_Pipeline_Project_2\target\surefire-reports\TEST-in.javahome.myweb.controller.CalculatorTest.xml $WORKSPACE'
		junit "/target/surefire-reports/*.xml"
		// sh "mvn jacoco:check"
                // jacoco ()
            }
        }
	stage('Code Coverage') {
	   steps{
                sh "mvn jacoco:report"
           }
	}
        stage('SonarQube analysis') {
	   steps{
                withSonarQubeEnv('sonarqube-9.7') {
                sh "mvn sonar:sonar"
        	}
           }
	}
        stage("Upload in Artifactory") {
            steps {
                rtUpload (
                    serverId: 'jfrog-artifact',
                    spec: '''{
                        "files": [
                            {
                                "pattern": "*.war",
                                "target": "java-demo-repo"
                            }
                        ]
                    }''',
		    "buildNumber": "${env.BUILD_NUMBER}",
		    "buildName": "${env.JOB_NAME}"
                )
            }
	}
	stage('deploy') { 
            steps {
                sh "cp "/target/"*.war ${tomcatweb}/*.war"
            }
	}
    }
}
