node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
        sh 'echo "build start"
       app = docker.build("earlyspring/test")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger earlyspring-cd-pipeline') {
                echo "triggering updatemanifestjob"
                build job: 'earlyspring-cd-pipeline', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
