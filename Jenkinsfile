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
    stage('Build')
    {
      steps{
        echo "Building"
      }
    }
  }
}
