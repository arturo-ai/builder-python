pipeline {
  agent {
    label "jenkins-jx-base"
  }
  environment {
    ORG = 'amfamlabs'
    APP_NAME = 'builder-python'
  }
  stages {
    stage('CI Build and push snapshot') {
      when {
        branch 'PR-*'
      }
      steps {
        container('jx-base') {
          sh "printenv"
          sh "docker build -t $DOCKER_REGISTRY/$ORG/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER ."
          sh "docker push $DOCKER_REGISTRY/$ORG/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER"
        }
      }
    }

    stage('Build and Push Release') {
      when {
        branch 'master'
      }
      steps {
        container('jx-base') {
          sh "jx step git credentials"
          sh "./jx/scripts/release.sh"
        }
      }
    }
  }
}
