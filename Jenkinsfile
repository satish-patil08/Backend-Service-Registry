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
                    sudo rm -rf /var/lib/jenkins/backend/service-registry/*
                    sudo cp -r target/*.jar /var/lib/jenkins/backend/service-registry/
                    sudo chown -R azureuser:azureuser /var/lib/jenkins/backend/service-registry/
                    sudo mv /var/lib/jenkins/backend/service-registry/*.jar /var/lib/jenkins/backend/service-registry/app.jar
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
