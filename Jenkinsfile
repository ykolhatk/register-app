pipeline
{
  agent { label 'Jenkins-Agent'}

  tools
  {
      mvn 'Maven3'
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
        echo "Building"
    }
  }
}
