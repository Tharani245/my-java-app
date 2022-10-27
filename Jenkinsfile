pipeline {
    agent any
    stages {
        stage("git clone") {
            steps {
                git branch: 'master',url: 'https://github.com/Tharani245/simple-java-project.git'
            }
        }
        stage("build") {
            steps {
                sh "mvn clean install"
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
