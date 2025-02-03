pipeline {
  agent any
  stages {
    stage('Generate Jar Files') {
      steps {
        sh '''mvn clean install'''
      }
    }
    stage('Delete the old containers') {
      steps {
        sh '''
         mkdir -p ~/backend/backend-users/
        sudo systemctl stop backend-service-registry
        sudo rm -rf ~/backend/backend-service-registry/*
        sudo cp -r target/*.jar ~/backend/backend-service-registry/
        sudo chown -R azureuser:azureuser ~/backend/backend-service-registry/
        sudo mv ~/backend/backend-service-registry/*.jar ~/backend/backend-service-registry/app.jar'''
      }
    }
    stage('Run the updated dockers') {
      steps {
        sh '''sudo systemctl start backend-service-registry'''
      }
    }
  }
}
