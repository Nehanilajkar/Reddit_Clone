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
    stage('Clean Workspace')
    {
	    steps{cleanWs()}
    }
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
    stage('Quality Gates')
    {
      steps
      { 
        script{waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube_token'}
      }  
    }
    stage('Install Dependencies')
    {
        steps {
                sh "npm install"
        }
    }
    stage('TRIVY FS SCAN') 
    {
        steps {
            sh "trivy fs . > trivyfs.txt"
         }
     }
    stage("Build Docker Image")
    {
         steps {
           sh 'docker build -t reddit:v1 .'
         }
    }
    stage('Docker Image Push')
    {
	 steps
	    {
	withCredentials([usernamePassword(credentialsId: 'DockerHub_token', passwordVariable: 'password', usernameVariable: 'username')]) {
		sh 'docker login -u $username -p $password'
		sh 'docker tag reddit:v1 krira259/reddit:v1'
		sh 'docker push krira259/reddit:v1'
		}
	    }
    }
  }
}
