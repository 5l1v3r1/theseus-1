pipeline {
  agent none // also forces each stage section contain its own agent section

  environment {
    GCLOUD_AUTH_BASE64 = credentials('GCLOUD_AUTH_BASE64')
  }

  stages {
    stage('Checkout') {
      agent {
        docker { image 'google/cloud-sdk:latest' }
      }
      steps {
        cleanWs()
        checkout scm
      }
    }
    stage('Test') {
      agent {
        docker { image 'google/cloud-sdk:latest' }
      }
      environment {
        DEFAULT_PROJECT = 'controlplane-dev-2'
        DEFAULT_ZONE = 'europe-west2-c'
        PERSIST_CLUSTER = '1'
      }
      steps {
        sh "mkdir -p /tmp/jq/ && curl https://github.com/stedolan/jq/releases/download/jq-1.5/jq-linux64 -Lo /tmp/jq/jq && chmod +x /tmp/jq/jq;"

        sh 'HOME=$(pwd); PATH="${PATH}:/tmp/jq/"; ./test/test-remote.sh; '
      }
    }
  }
}
