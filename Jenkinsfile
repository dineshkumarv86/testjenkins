pipeline {
  agent any
  stages {
    stage('1') {
      parallel {
        stage('1') {
          steps {
            echo 'hi'
            sleep 2
          }
        }

        stage('') {
          steps {
            echo '2'
          }
        }

      }
    }

    stage('') {
      steps {
        sleep 2
      }
    }

  }
}