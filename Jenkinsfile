pipeline{
    agent { label 'JDK_17'}
    triggers { pollSCM ('H/30 * * * *') }
    parameters {
        string(name: 'MAVEN_GOAL', defaultValue: 'package', description: 'maven goal') 
        }

    }
    stages{
        stage('VCS'){
            //agent { label 'git'}
            steps {
               git url: 'https://github.com/Vishnuchowki/game-of-life.git',
                    branch:'declarative' 
            }
        }
        stage('package'){
            //agent { label 'maven' || 'maven-jdk'} 
                tools{
                jdk 'JDK_8_UBUNTU'
            }
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('postbuild'){
            //agent { label 'k8s' && 'developer'}
            steps { 
                archiveArtifacts artifacts: '**/target/gameoflife.war',
                    onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
        //('post build') {
            //steps {
              //  archiveArtifacts artifacts: '**/target/gameoflife.war',
                //                 onlyIfSuccessful: true
               // junit testResults: '**/surefire-reports/TEST-*.xml'
          //  }
        //}
    }


