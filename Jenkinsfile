pipeline {
  agent any
  stages {
    stage('Checkout Code') {
      steps {
        sh 'git(url: \'https://github.com/EranFass/hello-world-war.git\', branch: \'dev\', changelog: true)'
      }
    }

  }
}