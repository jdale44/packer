pipeline {
    agent any
    stages {
        stage("AWS Demo") {
            steps {
                withCredentials([
                    [
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws_credential',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]
                ]) {
                    sh "aws s3 ls"
                }
            }
        }
        stage("scanning code") {
	    steps {
		 withSonarQubeEnv(installationName: 'Sonarcloud',credentialsId: 'jenkins') {
			 sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.8.7:sonar'
                }
	   }
       }
       stage("Building AMI") {
            steps {
                withCredentials([
                    [
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws_credential',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]
                ]) {
                    sh "packer init aws-ami-v1.pkr.hcl"
                    sh "packer build aws-ami-v1.pkr.hcl"
					
		}
             }				
	}
    }
}
