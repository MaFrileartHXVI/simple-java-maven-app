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
    stage('Manual Approval') {
        input message: "Lanjutkan ke tahap Deploy?"
    }
    stage('Deploy') {
        docker.image('maven:3.9.9-eclipse-temurin-21').inside('-u root -v /var/jenkins_home/.m2:/root/.m2') {
            sh '''
            # Jalankan aplikasi Java secara background
            nohup java -jar target/my-app-1.0-SNAPSHOT.jar > app.log 2>&1 &
            
            # Jeda pipeline selama 1 menit (sesuai Kriteria 3)
            sleep 60
            
            # Matikan aplikasi Java secara otomatis setelah 1 menit
            pkill -f my-app-1.0-SNAPSHOT.jar || true
            '''
        }
    }
}
