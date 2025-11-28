node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("adamumj/test")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    
    stage('Trigger Python-project') {
                //Run the job
                echo "triggering python2"
                build job: 'python2', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
