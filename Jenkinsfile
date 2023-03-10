pipeline{
    agent { label 'JDK_17'}
    triggers { pollSCM ('H/30 * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean package'], description: ' trying for choice parameters') 
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
        stage('notification'){           
            steps ('success') {
                mail to:"vc@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Yay, we passed."
                        }
                   
            steps ('failure') {
                mail to:"vc@gmail.co", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Boo, we failed."
                    }
            
            }
        }

    
}   
