node {
    // Jenkins Global Tool Configuration must include both Maven and a Java 11 JDK.
    def javaHome = tool 'JDK 11'
    def mvnHome = tool 'maven-3.9.6'
    def dockerImageTag = "shakirdocker/myapplication:${env.BUILD_NUMBER}"
    def remoteHost = '172.31.26.207'
    def remoteUser = 'root'

    env.JAVA_HOME = javaHome
    env.PATH = "${javaHome}/bin:${mvnHome}/bin:${env.PATH}"
    env.MAVEN_OPTS = "--add-opens jdk.compiler/com.sun.tools.javac.api=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.code=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.comp=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.file=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.main=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.model=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.parser=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.processing=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.tree=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.util=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.jvm=ALL-UNNAMED"

    stage('Clone Repo') {
      git 'https://github.com/DevSecOpsG/DevOps-Example.git'
    }

    stage('Build Project') {
      sh 'java -version'
      sh 'mvn -version'
      sh 'mvn clean install'
    }

    stage('Build Docker Image') {
      sh "docker build -t ${dockerImageTag} ."
    }

    stage('Docker login and push') {
      echo "Docker Image Tag Name: ${dockerImageTag}"
      sh 'docker images'
      withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
        sh "docker push ${dockerImageTag}"
      }
    }

    stage('Deploy to Remote Server') {
      withCredentials([
        usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS'),
        sshUserPrivateKey(credentialsId: 'remote-server-ssh', keyFileVariable: 'REMOTE_KEY', usernameVariable: 'REMOTE_USER')
      ]) {
        sh """
          ssh -o StrictHostKeyChecking=no -i \"\$REMOTE_KEY\" \"\$REMOTE_USER@${remoteHost}\" \
          \"docker login -u '\$DOCKER_USER' -p '\$DOCKER_PASS' && \
          docker pull ${dockerImageTag} && \
          docker stop devops-demo >/dev/null 2>&1 || true && \
          docker rm -f devops-demo >/dev/null 2>&1 || true && \
          docker run -d --name devops-demo -p 8080:8080 ${dockerImageTag}\" 
        """
      }
    }
}
