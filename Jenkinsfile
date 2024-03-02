pipeline
{
  agent { label 'Jenkins-Agent'}

  tools
  {
      maven 'Maven3'
      jdk 'Java17'
  }

  stages
  {
    stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
    stage('Checkout')
    {
      steps
      {
          git branch: 'main', credentialsId: 'github', url: 'https://github.com/ykolhatk/register-app.git'  
      } 
    }
    stage('Build')
    {
      steps{
        echo "Building"
        sh "mvn clean package"
      }
    }

    stage("Test Application"){
      steps {
                sh "mvn test"
           }
       }
    stage("SonarQube Analysis"){
           steps {
		        withSonarQubeEnv(credentialsId: 'jenkins-sonar-token') { 
                        sh "mvn sonar:sonar"
		        }
	           	
           }
       }
  }
 
}
