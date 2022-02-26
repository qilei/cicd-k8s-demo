pipeline {
  environment {
    registry = "qilei2010/cicd-k8s-demo"
    registryCredential = 'b714b759-4cfc-4d73-b35c-e99722645945'
    dockerImage = ''
  }
  tools {
      maven 'MAVEN_HOME'
  }
  agent any
  stages {
    stage('Compile') {
      steps {
        git 'https://github.com/qilei/cicd-k8s-demo.git'
        script{
                def mvnHome = tool name: 'MAVEN_HOME', type: 'maven'
                sh "${mvnHome}/bin/mvn package"
        }
      }
    }
    stage('Building Docker Image') {
      steps{
        sh "docker build --no-cache -t qilei2010/cicd-k8s-demo:0.2.0 ."
      }
    }
    stage('Push Image To Docker Hub') {
      steps{
        script {
          /* Finally, we'll push the image with two tags:
                   * First, the incremental build number from Jenkins
                   * Second, the 'latest' tag.
                   * Pushing multiple tags is cheap, as all the layers are reused. */
          docker.withRegistry('https://registry.hub.docker.com', 'b714b759-4cfc-4d73-b35c-e99722645945') {
              dockerImage.push("${env.BUILD_NUMBER}")
              dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploy to Kubernetes'){
        steps{
            sh 'kubectl apply -f deployment.yml'
       }
    }
  }
}
