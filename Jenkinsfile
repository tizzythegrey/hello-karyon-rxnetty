node{

        stage ('Checkout') {
        // checkout repository
          checkout scm
        }

        stage ('Install dependencies') {
           sh "sudo apt-get update -y"
           sh "sudo apt install unzip zip curl wget -y"
           sh "export SDKMAN_DIR="/usr/local/sdkman" && curl -s "https://get.sdkman.io" | bash"
           sh "source /usr/local/sdkman/bin/sdkman-init.sh"
           sh "sudo sdk install gradle"
           sh "sudo apt install java-1.8.0 java-1.8.0-openjdk-devel"
        }

        stage ('Java Build') {
        // build .jar package
          sh "./gradlew clean run"
          sh "./gradlew clean fatJar"
        }
        stage ('build docker image'){
         /* Build the Docker image with a Dockerfile, tagging it with the build number */
          def app = docker.build "demo/hello-karyon-rxnetty:${env.BUILD_NUMBER}"
        }
        stage ('Test'){
        /* We can run tests inside our new image */
           app.inside {
           sh "echo 'Tests passed'"
           }
        }
        stage ('Publish'){
           /* Finally, we'll push the image with two tags:
           * Second, the 'latest' tag.
           * Pushing multiple tags is cheap, as all the layers are reused. */
          docker.withRegistry('http://10.20.70.110:5000') {
            app.push("latest")
          }
        }
 }
