pipeline
{
  agent any
  tools
  { 
    jdk "jdk17"
    nodejs "node16"
  }
  environment
  {
    SCANNER_HOME = tool 'sonar-scanner'
  }
  stages{
    stage('Code Checkout')
    {
      steps
      {
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Nehanilajkar/Reddit_Clone.git']])
      }
    }
    stage('Code Analysis')
    {
      steps
      { 
        withSonarQubeEnv('sonarqube_server') {
         sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Redditclone-CI \
                    -Dsonar.projectKey=Redditclone-CI'''
        }
      }
    }
  }
}
