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
        stage('copy build'){
            steps {
                sh 'mkdir -p /tmp/$JOB_NAME/${BUILD_ID} && cp ./gameoflife-web/target/gameoflife.war /tmp/$JOB_NAME/${BUILD_ID}/'
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
        
    }
    post {
        success {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is success",
                body: "Use this URL ${BUILD_URL} for more info",
                to: 'vc@gmail.com',
                from: 'devops@qt.com'
        }
        failure {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is failed",
                body: "Use this URL ${BUILD_URL} for more info",
                to: "${GIT_AUTHOR_EMAIL}",
                from: 'devops@qt.com'
        }
    }   
    
}   