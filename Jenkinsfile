pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java21'
        maven 'Maven3'
    }
   
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/harashkapoor/register-app'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }
		stage("SonarQube analysis") {
    steps {
        script {
            withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                sh "mvn sonar:sonar"
            }
        }
    }
}

stage("Quality Gate") {
    steps {
        script {
            waitForQualityGate abortPipeline: false,
                credentialsId: 'jenkins-sonarqube-token'
        }
       }
     }
	}	
  }      


       
