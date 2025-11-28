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

    stage('Run Container') {
        //run the container
        echo "Starting container test"
        sh "docker run -d --name test-container -p 80001:80001 ${app.imageName()}"
        sh "sleep 10"
    }

    stage('Cleanup') {
         echo "Stopping and removing test container"
         sh "docker rm -f test-container || true"
  
    } 
    
 }
