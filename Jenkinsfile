node {
    // reference to maven
    // ** NOTE: This 'maven-3.9.6' Maven tool must be configured in the Jenkins Global Configuration.   
    def mvnHome = tool 'maven-3.9.6'
    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"
    
    stage('Clone Repo') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/shakirmir/DevOps-Example.git'
      // Get the Maven tool.
      // ** NOTE: This 'maven-3.9.6' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven-3.9.6'
    }    
  
    stage('Build Project') {
      // build project via maven
      sh "'${mvnHome}/bin/mvn' clean install"
    }
		
    stage('Build Docker Image') {
      // build docker image using shell command
      sh "docker build -t devopsexample:${env.BUILD_NUMBER} ."
    }
   	  
    stage('Deploy Docker Image and login'){
      
      echo "Docker Image Tag Name: ${dockerImageTag}"
	  
        sh "docker images"
        sh "docker login -u vickeyyvickey -p Hello@123" // put PWD
	
}
    stage('Docker push'){
       // docker images | awk '{print $3}' | awk 'NR==2'
	// sh "docker images | awk '{print $3}' | awk 'NR==2'"
	//sh echo "Enter the docker lattest imageID"
	//sh "read imageid"
	   sh "docker tag 90cc3c109088   vickeyyvickey/myapplication" //must change your name and tag no
        sh "docker push   vickeyyvickey/myapplication"
  }
}
