pipeline {
  environment {
    registry = "peppe2794/${JOB_NAME}"
    registryCredential = 'dockerhub'
    dockerImage = ''
    DOCKER_TAG = getVersion().trim()
    IMAGE="${JOB_NAME}"
  }
  //If is a nodejs application
  tools {
    nodejs 'NodeJS'
 }
  agent any
  stages {
    stage('SonarQube analysis'){
      steps{
        sh 'echo SonarQube analysis'
        /*
        withSonarQubeEnv(installationName: 'Sonarqube2', credentialsId: 'Sonarqube2') {
          sh "${tool("sonar_scanner")}/bin/sonar-scanner"
        }*/
      }
    }
    stage('Building image') {
      steps{
        sh 'echo Building image'
        /*
        script {
          dockerImage = docker.build("$registry:$DOCKER_TAG")
        }*/
        
      }
    }
    stage('Static Security Assesment'){
      steps{
        sh ' echo Static Security Assesment'
        /*
        sh 'docker run --name ${IMAGE} -t -d $registry:${DOCKER_TAG}'
        sh 'inspec exec https://github.com/dev-sec/linux-baseline -t docker://${IMAGE} --reporter html:Results/Linux_Baseline_report.html --chef-license=accept || true'
        sh 'inspec exec https://github.com/dev-sec/apache-baseline -t docker://${IMAGE} --reporter html:Results/Apache_Baseline_report.html --chef-license=accept || true'
        sh 'inspec exec https://github.com/dev-sec/php-baseline -t docker://${IMAGE} --reporter html:Results/php_Baseline_report.html --chef-license=accept || true'
        sh 'docker stop ${IMAGE}'
        sh 'docker container rm ${IMAGE}'
        
        withCredentials([usernamePassword(credentialsId: 'GIT', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {        
          
          sh 'git remote set-url origin "https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/${JOB_NAME}.git"'
          sh 'git add Results/*'
          sh 'git commit -m "Add report File"'
          sh 'git push origin HEAD:main'
          
        }*/
     }
    }
    stage('Push Image') {
      steps{
        script {
          sh ' echo Push Image'
          /*
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }*/
        }
      }
    }
 }
    post{
    success{
      echo 'Post success'
      build job: 'SecDevOpsFlowTemplate_WordpressExample', parameters: [string (value: "peppe2794/"+"$IMAGE"+":"+"$DOCKER_TAG", description: 'Parametro', name: 'WP')]
    }
  }
}

def getVersion(){
  def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
  return commitHash
}
