pipeline {
  agent any
  environment {
    CLOUDSDK_CORE_PROJECT='ybvr-dev'
    CLIENT_EMAIL='jenkins@ybvr-dev.iam.gserviceaccount.com'
    GCLOUD_CREDS=credentials('gcloud-ybvr-dev')
  }
  stages {
    stage('Verify version') {
      steps {
        sh '''
          gcloud version
        '''
      }
    }
    stage('Authenticate') {
      steps {
        sh '''
          gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
        '''
      }
    }
    // stage('Pull repo') {
    //   steps {
    //     sh 'git pull git@github.com:igdom/jenkins-cloud-run.git'
    //   }
    // }
    // stage('Install service') {
    //   steps {
    //     sh '''
    //       gcloud run services replace jenkins-cloud-run/service.yaml --platform='managed' --region='europe-west1-b'
    //     '''
    //   }
    // }
    stage('Install service') {
      steps {
        sh '''
          gcloud run services replace service.yaml --platform='managed' --region='europe-west1-b'
        '''
      }
    }
    stage('Allow allUsers') {
      steps {
        sh '''
          gcloud run services add-iam-policy-binding hello --region='europe-west1-b' --member='allUsers' --role='roles/run.invoker'
        '''
      }
    }
  }
  post {
    always {
      sh 'gcloud auth revoke $CLIENT_EMAIL'
    }
  }
}
