pipeline {
    agent any
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
	stage("Test and Code Coverage") {
            steps {
                sh "mvn test"
                // junit allowEmptyResults: true, testResults: '**/test-results/*.xml'
                // junit '**/test-results/*.xml'
		// sh 'ln -s C:\Users\Shanmugam K\.jenkins\workspace\Declarative_Pipeline_Project_2\target\surefire-reports\TEST-in.javahome.myweb.controller.CalculatorTest.xml $WORKSPACE'
		junit "/target/surefire-reports/*.xml"
		sh "mvn jacoco:check"
                jacoco ()
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
                )
            }
	}
    }
}
