node{

        stage ('Checkout') {
        // checkout repository
          checkout scm
        }

        stage ('Install dependencies') {
           sh "sudo apt-get update -y"
           sh "sudo apt-get install unzip zip curl wget -y"
        }

        stage ('Java Build') {
        // build .jar package
          sh "./gradlew clean fatJar"
        }
        stage ('build docker image'){
         /* Build the Docker image with a Dockerfile, tagging it with the build number */
         /* def app = docker.build "demo/hello-karyon-rxnetty:${env.BUILD_NUMBER}" */
          sh "sudo docker build -t demo/hello-karyon-rxnetty:${env.BUILD_NUMBER} ."
          sh "sudo docker tag http://10.20.70.175:5000/demo/hello-karyon-rxnetty:${env.BUILD_NUMBER}"
        }
        
        /* stage ('Test'){ */
        /* We can run tests inside our new image */
         /*  app.inside { */
         /*  sh "echo 'Tests passed'" */
          /* } */
       /* } */
       
        stage ('Publish'){
           sh "sudo docker push http://10.20.70.175:5000/demo/hello-karyon-rxnetty:${env.BUILD_NUMBER}"
        
        }
 } 
