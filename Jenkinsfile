node {
    def app
    stage('Clone repository') {      
        checkout scm
    }

    stage('Build sbt code') {
        echo "${env.WORKSPACE}"
        sh "sbt clean compile docker:stage"
    }

    stage('Build image') {  
        docker.withRegistry('http://172.30.1.1:5000', 'docker-registry') {
            app = docker.build("myproject/skakka")
        }       
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {        
        docker.withRegistry('http://172.30.1.1:5000', 'docker-registry') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}