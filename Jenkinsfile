node {
  node('cordova') {

      def buildNumber = env.BUILD_NUMBER
      def workspace = env.WORKSPACE
      def buildUrl = env.BUILD_URL


      // PRINT ENVIRONMENT TO JOB
      echo "workspace directory is $workspace"
      echo "build URL is $buildUrl"
      echo "build Number is $buildNumber"
      echo "PATH is $env.PATH"

      try {
        stage('Checkout') { 
           checkout scm
        }
  
        stage('Build') {
           sh "npm install"
        }
  
        stage('Test') {
           sh "PLATFORM=android npm run test"
           sh "PLATFORM=ios npm run test"
        }
  
        stage('Publish NPM snapshot') {
           def currentVersion = sh(returnStdout: true, script: "npm version | grep \"{\" | tr -s ':'  | cut -d \"'\" -f 4").trim()
           def newVersion = "${currentVersion}-${buildNumber}"
           sh "npm version ${newVersion} --no-git-tag-version && npm publish --tag next"
        }
        
      } catch (e){
         mail subject: 'Error on build', to: 'contact@martinreinhardt-online.de'
         throw e
      }
   }

}