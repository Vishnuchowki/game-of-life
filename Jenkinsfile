pipeline{
    #agent { label 'JDK_17'}
    stages{
        stage(VCS){
            //agent { label 'git'}
            steps {
               git url: 'https://github.com/Vishnuchowki/game-of-life.git'
                branch: 'declarative' 
            }
        }
        stage(package){
            //agent { label 'maven' || 'maven-jdk'}
            steps {
                sh 'export PATH="/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin:$PATH" && mvn package'
            }
        }
        stage(postbuild){
            //agent { label 'k8s' && 'developer'}
            steps { 
                archiveArtifacts artifacts: '**/target/gameoflife.war',
                    onlyIfSuccessful: true
                junit testResults: '**/surefire-report/TEST-*.xml'
            }
        }
    }


}