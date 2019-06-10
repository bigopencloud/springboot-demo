node {

   env.JAVA_HOME = tool 'Openjdk-1.8.0'
   def mvnHome = tool 'M3'
   env.PATH = "${mvnHome}/bin:${env.JAVA_HOME}/bin:${env.PATH}"
   
   // Create an Artifactory server instance
   //def server = Artifactory.server('ArtifactoryServer')

   // Mark the code checkout 'stage'....
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/bigopencloud/springboot-demo.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
   }

   // Mark the code build 'stage'....Run the maven build
    stage ('Build the Source Code') {
      
    stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }

    }

   stage ('Build The Docker Image') {
   //def dockerBuildInfo = rtMaven.run pom: 'pom.xml', goals: 'docker:build'
   
   stage 'Run the Docker Container created'

    echo "workspace =${workspace}"

    echo "Job Name is ${env.JOB_NAME} and build number is ${env.BUILD_NUMBER}"

    def imageName = "boc/demo-0.1.${env.BUILD_NUMBER}"
    
    echo "building the docker image ${imageName}"

    sh "docker build -f src/main/docker/Dockerfile -t ${imageName} ."
   }

    echo "It appears that build url ${env.BUILD_URL} is success"
      
}