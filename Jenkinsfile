node {
    def mvnHome = tool 'maven-3.5.2'
    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"
    
    stage('Clone Repo') {
        checkout scm
    }    
    
    stage('Build Project') {
        bat "\"${mvnHome}\\bin\\mvn\" -B -DskipTests clean package"
    }
    
    stage('Build Docker Image') {
        withDockerServer([uri: 'tcp://192.168.8.100:2375']) {
            docker.build("devopsexample:${env.BUILD_NUMBER}")
        }
    }
    
    stage('Deploy Docker Image') {
        withDockerServer([uri: 'tcp://192.168.8.100:2375']) {
            docker.image("devopsexample:${env.BUILD_NUMBER}").run("-p 2222:2222", "--name devopsexample -d")
        }
    }
}
