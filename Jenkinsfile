pipeline {
  agent any
  stages {
    stage('检出') {
      agent any
      steps {
        checkout([$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]],
        userRemoteConfigs: [[url: env.GIT_REPO_URL, credentialsId: env.CREDENTIALS_ID]]])
      }
    }
    stage('构建 APK') {
      agent any
      steps {
        sh '''ls $ANDROID_SDK_ROOT/build-tools
ls $ANDROID_SDK_ROOT/platforms
pwd
ls
./gradlew assembleDebug

mkdir output

cp app/build/outputs/apk/debug/* output
pwd'''
      }
    }
    stage('归档 APK') {
      agent any
      steps {
        codingArtifactsGeneric(files: 'output/*', repoName: env.DEPOT_NAME, version: env.GIT_BUILD_REF, credentialsId: '${env.CODING_ARTIFACTS_CREDENTIALS_ID}', withBuildProps: true, zip: '${env.JOB_ID}/${env.CI_BUILD_ID}/android_app.zip')
      }
    }
  }
  environment {
    ARTIFACT_IMAGE = "${ARTIFACT_BASE}/${PROJECT_NAME}/${DEPOT_NAME}/${DEPOT_NAME}"
  }
}