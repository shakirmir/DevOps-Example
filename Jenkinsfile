node {
    // reference to maven
    // ** NOTE: This 'maven-3.9.6' Maven tool must be configured in the Jenkins Global Configuration.   
    def mvnHome = tool 'maven-3.9.6'
    def dockerImageTag = "shakirdocker/myapplication:${env.BUILD_NUMBER}"
    
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
      sh "docker build -t ${dockerImageTag} ."
    }
   	  
    stage('Deploy Docker Image and login'){
      
      echo "Docker Image Tag Name: ${dockerImageTag}"
	  
        sh "docker images"
        withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
        }
	
}
    stage('Docker push'){
       sh "docker push ${dockerImageTag}"
  }
}
