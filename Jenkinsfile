 pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh './mvnw clean compile'
            }
        }
	 	stage('Unit Test') {
		    steps {
		        sh './mvnw test'
		    }
		    post {
		        always {
		            junit 'target/surefire-reports/*.xml'
            		step([ $class: 'JacocoPublisher' ])
		        }
		    }
		}
	 	stage('Static Code Analysis') {
		    steps {
		        configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
		            sh './mvnw sonar:sonar -s $MAVEN_SETTINGS'
		        }
		    }
		}
        stage('Package') {
            steps {
                sh "./mvnw package -DskipTests"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

    }
}