pipeline{
    agent { label 'JDK_17'}
    triggers { pollSCM ('* * * * *') }
    parameters {
        choice(name: 'ami_filter_name', choices: 'name', description: 'assigning the name')
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
        stage('launch_instances'){
            steps {
                
                        sh '''
                            #!/bin/bash
                            aws ec2 describe-images --filters "Name={params.ami_filter_name},Values={params.ami_filter_Value}" --query "Images[0].ImageId
                            ami_id=$(aws ec2 describe-images --filters "Name={params.ami_filter_name},Values={params.ami_filter_Value}" --query "Images[0].ImageId)
                            vpc_id=$(aws ec2 describe-vpcs --filters "Name=is-default,Values=true" --query "Vpcs[0].VpcId" --output text)
                            sg_id=$(aws ec2 create-security-group --group-name {params.security_group_id} --description "open 22 and 80" --vpc-id $vpc_id --output text --query "GroupId")
                            aws ec2 authorize-security-group-ingress --group-id $sg_id --protocol tcp --port 22 --cidr '0.0.0.0/0'
                            aws ec2 authorize-security-group-ingress --group-id $sg_id --protocol tcp --port 80 --cidr '0.0.0.0/0'
                            instace_id=$(aws ec2 run-instances --image-id $ami_id --instance-type {params.instance-type} --key-name {params.ami_keypair_name} --security-group-ids $sg_id --associate-public-ip-address --query "Instances[0].InstanceId" --output text)
                            aws ec2 describe-instances --instance-ids $instace_id --query "Reservations[0].Instances[0].PublicIpAddress"
                            public_ip=$(aws ec2 describe-instances --instance-ids $instace_id --query "Reservations[0].Instances[0].PublicIpAddress" --output text)
                            echo $public_ip
                            ssh ubuntu@$public_ip
                            sudo apt update && sudo apt install apache2 -y
                            exit
                            echo "http://${public_ip}"
                        '''
            }    
        }
     
    }
  
}   
