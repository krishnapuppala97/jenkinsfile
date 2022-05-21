pipeline{
    agent any
    triggers{
        pollSCM('* * * * *')
    }
   tools{
     maven 'pk'
    } 
    stages{
        stage('chechkout'){
            steps{
            git branch: 'development', url: ' https://github.com/krishnapuppala97/maven-web-application.git'
            }
        }
        stage('build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('build docker images'){
        steps{
            sh 'docker build -t krishnapuppala97/mynewimage:$BUILD_NUMBER . '
        }
     }
     stage('docker image push'){
         steps{
             withCredentials([string(credentialsId: '82ae06b7-4adb-40cc-ab6b-37380b614b1f', variable: 'dockerpasswd')]) {
              sh 'docker login -u krishnapuppala97 -p ${dockerpasswd} '
              sh 'docker push  krishnapuppala97/mynewimage:$BUILD_NUMBER'
         }
     }
    }
    stage('depoly docker container'){
        steps{
            sshagent(['8fd95775-f6d3-4749-8c2e-d8ad442abfc9']) {
    sh ' ssh -o StrictHostKeyChecking=no ubuntu@172.31.31.35 docker rm -f krishnapuppala || ture '
    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.31.35 docker run --name krishnapuppala -d -p 1234:8080 krishnapuppala97/mynewimage:$BUILD_NUMBER  '
}
        }
    }
 }
}
