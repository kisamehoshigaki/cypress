pipeline {
  agent {
    // this image provides everything needed to run Cypress
    docker {
      image 'cypress/browsers:node11.13.0-chrome73'
    }
  }

  stages {
       stage('build') {
        steps {
          // there a few default environment variables on Jenkins
          // on local Jenkins machine (assuming port 8080) see
          // http://localhost:8080/pipeline-syntax/globals#env
          echo "Running build ${env.BUILD_ID} on ${env.JENKINS_URL}"
          sh 'npm install'
          sh 'npm ci'
          sh 'npm run cy:verify'
        }
    }
      
    stage('build and test') {
    //   environment {
        // we will be recording test results and video on Cypress dashboard
        // to record we need to set an environment variable
        // we can load the record key variable from credentials store
        // see https://jenkins.io/doc/book/using/using-credentials/
        // CYPRESS_RECORD_KEY = credentials('cypress-record-key')
    //   }

      steps {
        sh "npm run cypress:run"
      }
    }
  }

    post {
        always {
            archiveArtifacts artifacts: 'cypress/videos/*', fingerprint: true
        }
    }
}
