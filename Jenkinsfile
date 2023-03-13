pipeline{
    agent { label 'JDK_17'}
    triggers { pollSCM ('* * * * *') }
    parameters {
        string(name: 'ami_filter_name', defaultValue: 'name', description: 'assigning the name')
        choice(name: 'ami_filter_Value', choices: ['ubuntu/images/hvm-ssd/ubuntu-jammy-22.04*', 'ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-*'],description: 'providing options for different ubunto version')
        choice(name: 'ami_keypair_name', choices: ['id_rsa', 'dock', 'my_key'],description: 'providing options for different keypares')
        choice(name: 'instance_type', choices: ['t2_micro', 't2_small', 't2_medium'],description: 'providing options for different types of instances')
        choice(name: 'security_group_id', choices: ['sg', 'sg1', 'sg2'],description: 'providing options for different types of security groups')
 
        }

    stages{
        stage('VCS'){
            //agent { label 'git'}
            steps {
               git url: 'https://github.com/Vishnuchowki/game-of-life.git',
                    branch:'awsexercise' 
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
        
    }
    
    
}   
