node {
    stage('Preparation') {
        checkout scm
    }
    stage('Build') {
        docker.image('maven:3.9.9-eclipse-temurin-21').inside('-u root -v /var/jenkins_home/.m2:/root/.m2') {
            sh 'mvn -B -DskipTests clean package'
        }
    }
    stage('Test') {
        docker.image('maven:3.9.9-eclipse-temurin-21').inside('-u root -v /var/jenkins_home/.m2:/root/.m2') {
            sh 'mvn test'
        }
    }
    stage('Deliver') {
        sh 'chmod +x ./jenkins/scripts/deliver.sh && ./jenkins/scripts/deliver.sh'
    }
}