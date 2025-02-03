pipeline {
  agent any
  stages {
    stage('Generate Jar Files') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Delete the old containers') {
      steps {
        sh '''
          sudo systemctl stop service-registry
          sudo rm -rf ~/backend/service-registry/*
          sudo cp -r target/*.jar ~/backend/service-registry/
          sudo chown -R azureuser:azureuser ~/backend/service-registry/
          sudo mv ~/backend/service-registry/*.jar ~/backend/service-registry/app.jar
        '''
      }
    }
    stage('Run the updated dockers') {
      steps {
        sh 'sudo systemctl start service-registry'
      }
    }
  }
}
