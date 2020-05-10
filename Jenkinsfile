pipeline {
        environment {
    registry = "rtkjain/calc-image"
    registryCredential = 'docker-hub-credentials'
    dockerImage = ''
    dockerImageLatest = ''
  }
      agent any
      stages {
            stage('Init') {
                  steps {
                        echo 'Hi, this is Ritik Jain'
                        echo 'I am executing calculator program via piipeline'
                  }
            }
                stage('Cloning Git') {
      steps {
        git 'https://github.com/rtkjain97/devops-cycle.git'
      }
    }

            stage('Build') {
                  steps {
                        sh 'mvn -f pom.xml clean package'
                  }

            }
            stage('Building image') {
      steps{
                sh "pwd"
                sh "ls -a"
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          dockerImageLatest = docker.build registry + ":latest"
        }
      }
    }   
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            dockerImageLatest.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }   
        stage('Execute Rundeck job') {
        steps {
          script {
            step([$class: "RundeckNotifier",
                  includeRundeckLogs: true,
                  jobId: "9bfa49a9-cb83-414a-9cd2-9bed52e07bee",
                  rundeckInstance: "Rundeck",
                  shouldFailTheBuild: true,
                  shouldWaitForRundeckJob: true,
                  tailLog: true])
            //echo "Rundeck JOB goes here"
          }
        }
    }
}
}
