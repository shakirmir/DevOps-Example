node {
    // Jenkins Global Tool Configuration must include both Maven and a Java 11 JDK.
    def javaHome = tool 'JDK 11'
    def mvnHome = tool 'maven-3.9.6'
    def dockerImageTag = "shakirdocker/myapplication:${env.BUILD_NUMBER}"

    env.JAVA_HOME = javaHome
    env.PATH = "${javaHome}/bin:${mvnHome}/bin:${env.PATH}"
    env.MAVEN_OPTS = "--add-opens jdk.compiler/com.sun.tools.javac.api=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.code=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.comp=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.file=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.main=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.model=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.parser=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.processing=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.tree=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.util=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.jvm=ALL-UNNAMED"

    stage('Clone Repo') {
      git 'https://github.com/shakirmir/DevOps-Example.git'
    }

    stage('Build Project') {
      sh 'java -version'
      sh 'mvn -version'
      sh 'mvn clean install'
    }

    stage('Build Docker Image') {
      sh "docker build -t ${dockerImageTag} ."
    }

    stage('Deploy Docker Image and login') {
      echo "Docker Image Tag Name: ${dockerImageTag}"
      sh 'docker images'
      withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
      }
    }

    stage('Docker push') {
      sh "docker push ${dockerImageTag}"
    }
}
