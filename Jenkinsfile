node {

   env.JAVA_HOME = tool 'Openjdk-1.8.0'
   def mvnHome = tool 'Maven-3.3.9'
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
      mvnHome = tool 'M3'
   }

   // Mark the code build 'stage'....Run the maven build
    stage 'Build the Source Code'
      
    // Create and set an Artifactory Maven Build instance:
    def rtMaven = Artifactory.newMavenBuild()
    rtMaven.resolver server: server, releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot'
    rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'
    
    // Optionally include or exclude artifacts to be deployed to Artifactory:
    rtMaven.deployer.artifactDeploymentPatterns.addInclude("frog*").addExclude("*.zip")
    
    // Set a Maven Tool defined in Jenkins "Manage":
    rtMaven.tool = 'M3'
    // Optionally set Maven Ops
    rtMaven.opts = '-Xms1024m -Xmx4096m'
    
    // Run Maven:
    def buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean package'

    
    // Publish the build-info to Artifactory:
    server.publishBuildInfo buildInfo
    //sh 'mvn clean package docker:build'

   stage 'Build The Docker Image'
   //def dockerBuildInfo = rtMaven.run pom: 'pom.xml', goals: 'docker:build'
   
   stage 'Run the Docker Container created'

    echo "workspace =${workspace}"

    echo "Job Name is ${env.JOB_NAME} and build number is ${env.BUILD_NUMBER}"

    def imageName = "boc/demo-0.1.${env.BUILD_NUMBER}"
    
    echo "building the docker image ${imageName}"

    sh "docker build -f src/main/docker/Dockerfile -t ${imageName} ."

    echo "It appears that build url ${env.BUILD_URL} is success"
      
}