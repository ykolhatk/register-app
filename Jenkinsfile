pipeline
{
  agent { label 'Jenkins-Agent'}

  tools
  {
      maven 'Maven3'
      jdk 'Java17'
  }
  environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "ykolhatk"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    //JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
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
		        withSonarQubeEnv(credentialsId: 'jenkins-sonar-token', installationName: 'sonarqube-server') { 
                        sh "mvn sonar:sonar"
		        }
	           	
           }
       }
    stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonar-token'
                }	
            }

        }
    stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

       }

    stage("Trivy Scan") {
           steps {
               script {
	            sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ykolhatk/register-app-pipeline:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
               }
           }
       }

       stage ('Cleanup Artifacts') {
           steps {
               script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
               }
          }
       }
  }
 
}
