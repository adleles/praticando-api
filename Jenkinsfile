pipeline {
  agent any
  stages {
    stage("verify tooling") {
      steps {
        bat '''
          docker version
          docker info
          docker compose version 
          curl --version
          '''
      }
    }
    stage('Start container') {
      steps {
        bat 'docker compose -f docker-compose.prod.yml up -d --no-color --wait'
        bat 'docker compose -f docker-compose.prod.yml ps'
      }
    }
    stage('Wait for container') {
      steps {
        sh 'sleep 15'
      }
    }
    stage('Run tests against the container') {
      steps {
        script {
          def containerIds = sh(returnStdout: true, script: 'docker compose -f docker-compose.prod.yml ps -q').trim().split('\n')
          def desiredContainerId = containerIds[0] 
          bat "docker exec '${desiredContainerId}' curl http://localhost:9090"
        }
      }
    }
  }
  
}