node {
    stage('SCM') {
        git 'https://github.com/wning13/test.git'
    }
    stage('QA') {
        //sh 'sonar-scanner'
        def sonarqubeScannerHome = tool name: 'SonarQubeScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
        sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -D sonar.host.url=http://192.168.11.88:9000"

    }
    stage('build') {
        def mvnHome = tool 'M3'
        sh "${mvnHome}/bin/mvn -B clean package"
    }
    stage('deploy') {
        sh "docker stop my || true"
        sh "docker rm my || true"
        sh "docker run --name my -p 11111:8080 -d tomcat"
        sh "docker cp target/test-0.0.1-SNAPSHOT.war my:/usr/local/tomcat/webapps"
    }
    stage('results') {
        archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
    }
}
