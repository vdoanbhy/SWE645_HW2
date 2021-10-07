pipeline {
  agent any
  environment {
    DOCKERHUB_PASS = credentials('docker-pass')
  }
  stages {
    stage ("Building the web app into a docker image"){
      steps {
        script {
          checkout scm
          sh 'rm -rf SWE645_Doan_HW1/src/main/webapp/*.war'
          sh 'jar -cvf studentSurvey.war -C SWE645_Doan_HW1/src/main/webapp/ .'
          sh 'mv studentSurvey.war SWE645_Doan_HW1/src/main/webapp/'
          sh 'echo ${BUILD_TIMESTAMP}'
          sh "docker login -u cparupal -p $DOCKERHUB_PASS_PSW"
          sh "docker build -t cparupal/studentsurvey645:${env.BUILD_ID} SWE645_Doan_HW1/src/main/webapp/"
          
        }
      }
    }
    
    stage("Pushing Image to Dockerhub"){
      steps{
        script{
          sh "docker push cparupal/studentsurvey645:${env.BUILD_ID}"
        }
      }
    }
    
    stage("Deploying image to cluster on Rancher"){
      steps{
        script{
          sh "kubectl set image deployment/studentsurvey studentsurvey=cparupal/studentsurvey645:${env.BUILD_ID} -n default"
        }
      }
    }

  }
}
