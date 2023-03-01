node('JDK_17'){ 
    stage('version control') {
        git url: 'https://github.com/Vishnuchowki/game-of-life.git',
            branch: 'scripted'
    }
    stage('build the code') {
        sh 'export PATH="/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin:$PATH" && mvn package'
    }
    stage('archive the artifacts') {
        archiveArtifacts artifacts: '**/target/gameoflife.war', 
            allowEmptyArchive: false
            }
    stage('show the test results') {
        junit testResults: '**/surefire-reports/TEST-*.xml',
            allowEmptyResults: true
         }
    
}

