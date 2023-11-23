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
    stage('Prune Docker data') {
      steps {
        bat 'docker system prune -a --volumes -f'
      }
    }
    stage('Start container') {
      steps {
        bat 'docker compose -f docker-compose.yml up -d --no-color --wait'
        bat 'docker compose -f docker-compose.yml ps'
      }
    }
    stage('Wait for container') {
      steps {
        bat 'sleep 30'
      }
    }
    stage('Run tests against the container') {
      steps {
        bat 'curl http://localhost:9090'
      }
    }
  }
  
}