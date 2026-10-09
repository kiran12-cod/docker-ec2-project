pipeline {
  agent any
  environment {
    IMAGE = "ghcr.io/kiran12-cod/docker-ec2-project:latest"
    PATH = "${env.PATH};C:\\Users\\kamal\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;C:\\Users\\kamal\\AppData\\Local\\Microsoft\\WinGet\\Packages\\Hashicorp.Terraform_Microsoft.Winget.Source_8wekyb3d8bbwe"
  }
  stages {
    stage('Build image') {
      steps { bat 'docker build -t %IMAGE% .' }
    }
    stage('Push to GHCR') {
      steps {
        withCredentials([string(credentialsId: 'ghcr-token', variable: 'TOKEN')]) {
          bat 'echo %TOKEN%| docker login ghcr.io -u kiran12-cod --password-stdin'
          bat 'docker push %IMAGE%'
        }
      }
    }
  }
}