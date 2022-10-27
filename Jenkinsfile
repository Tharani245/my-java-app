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
                junit '*.xml'
                // jacoco ()
            }
        }
        stage('SonarQube analysis') {
	   steps{
                withSonarQubeEnv('sonarqube-9.7') {
                sh "mvn sonar:sonar"
        	}
           }
        }
    }
}
