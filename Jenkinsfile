node {
    agent any
    
     environment {
        registry = "earlyspring/test"
        registryCredential = 'dockerhub'
    }

    stage('Clone repository') {
      

        checkout scm
    }

   stage('Building image') {
            steps{
              script {
                dockerImage = docker.build registry + ":V$BUILD_NUMBER"
              }
            }
        }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
             dockerImage.push("V$BUILD_NUMBER")
        }
    }
    
    stage('Trigger earlyspring-cd-pipeline') {
                echo "triggering earlyspring-cd-pipeline"
                build job: 'earlyspring-cd-pipeline', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
