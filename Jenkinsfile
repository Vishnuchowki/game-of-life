pipeline{
    agent { label 'JDK_17'}
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
                tolls{
                jdk 'JDK_8_UBUNTU'
            }
            steps {
                sh 'mvn package'
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