properties([parameters([string(defaultValue: '038856091169', description: 'Please provide your AWS account ', name: 'AWS_ACCOUNT', trim: true)])])

node {
    def REPOLOCATION = "https://github.com/Shakh0007/ec2.git"
    def BRANCHNAME = "main"
    
    stage("Clean Up") {
        sh "docker images"
        sh "docker image prune --force"
    }
    stage("Pull Docker Image"){
        sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com"
        sh "docker pull ${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com/tools"
    }
    withDockerContainer(image: '${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com/tools'){
        stage("Clone") {
            checkout([$class: 'GitSCM', branches: [[name: "*/${BRANCHNAME}"]], extensions: [], userRemoteConfigs: [[url: "${REPOLOCATION}"]]])
        }
    }
    withDockerContainer(image: '${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com/tools'){
        stage("Initialize") {
            sh "terraform init"
        }
    }
    withDockerContainer(image: '${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com/tools'){
        stage("Apply") {
            sh "terraform apply -auto-approve"
        }
    }
    stage("Email notification") {
        sh "echo hello"
    }
}



// node {
//     Authenticate, 
//     pull docker image
//      create container
//              Clone a repo
//      create container     
//              terraform init
//      create vault container
//              pull secrets
//      create container 
//              terraform apply
//      create container 
//              run packer
//      create sonarqube container
//              run sonarqube tasks
// }







// node {
//     Clone 
//     Code analysis   (sonarqube, veracode)
//     Build 
//     Tagging
//     Authentication 
//     Push 
//     Email 
// }