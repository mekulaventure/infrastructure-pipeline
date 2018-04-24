properties([pipelineTriggers([githubPush()])])

node('linux') {
    git url: 'https://github.com/mekulaventure/infrastructure-pipeline.git', branch: 'master'
    stage('GetInstances') {
        sh "aws ec2 describe-instances --region us-east-1"
    }
    stage('CreateInstance') {
        sh "aws ec2 run-instances --image-id ami-467ca739 --count 1 --instance-type t2.micro --key-name testkey --security-group-ids sg-c081f989 --subnet-id subnet-f114bdbb --region us-east-1"
    }
    stage('TerminateInstances') {
        sh "def output = sh returnStdout: true, script: 'aws ec2 describe-instances | jq .'"
        sh "aws ec2 wait instance-running(aws ec2 terminate-instances --instance-ids {{def}})"
    }
    stage('Test') {
        sh "env"
    }
}
