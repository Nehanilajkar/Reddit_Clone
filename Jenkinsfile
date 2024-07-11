pipeline
{
  agent any
  tools
  { 
    jdk "jdk17"
    nodejs "node16"
  }
  stages{
    stage('Code Checkout')
    {
      steps
      {
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Nehanilajkar/Reddit_Clone.git']])
      }
    }
  }
}
