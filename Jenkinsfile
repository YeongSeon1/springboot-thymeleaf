pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'build done'
        sh './gradlew clean build'
      }
    }

    stage('upload') {
      steps {
        echo 'upload done'
        sh 'aws s3 cp build/libs/application.war s3://springbootcicos3/application.war --region us-east-1'
      }
    }

    stage('deploy') {
      steps {
        echo 'deploy done'
        sh 'aws elasticbeanstalk create-application-version --region us-east-1 --application-name Springbootcicdyeongseon-env --version-label ${BUILD_TAG} --source-bundle S3Bucket="springbootcicos3",S3Key="application.war"'
        sh 'aws elasticbeanstalk update-environment --region us-east-1 --environment-name Springbootcicdyeongseonenv-env --version-label ${BUILD_TAG}'
        slackSend(color: '#FF0000', message: "빌드 완료: 성공 실패는 따로 확인!! STATUS: Job'${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      }
    }

  }
}