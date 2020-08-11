pipeline {
  agent {
    docker {
      image "bryandollery/terraform-packer-aws-alpine"
      args "-u root"
    }
    
  }
  
    environment{
        CREDS = credentials('aws_nuha_creds')
        AWS_ACCESS_KEY_ID="${CREDS_USR}"
        AWS_SECRET_ACCESS_KEY="${CREDS_PSW}"
        OWNER= "nuha"
        TF_NAMESPACE="nuha"
        PROJECT_NAME="web-server"
    }
  stages {
    stage("build") {
      packer build packer.json
    }
  }
  
  
stages{	
   stage('installDependinces') {
	steps{
		sh 'apk add make git'
	}
} 

   stage('cloneRepo'){
	    steps {
	sh 'rm -rf jenkins-packer || true '
	sh 'git clone https://github.com/nbaghanim/jenkins-lab-2-packer'	
	echo 'Done clonning'
	}
	}
   stage('addCredsfile'){
   steps {
    withCredentials([usernamePassword(
        credentialsId: '${aws_nuha_creds}',
        usernameVariable: 'aws_access',
        passwordVariable: 'aws_secret',
    )]){
	sh 'touch jenkins-packer/creds/credentials'
	sh "echo '[kh-labs]' >> ./jenkins-lab-2-packer/creds/credentials"
	sh 'echo aws_access_key_id="${aws_access}" >> ./jenkins-lab-2-packer/creds/credentials'
	sh 'echo aws_secret_access_key="${aws_secret}" >> ./jenkins-lab-2-packer/creds/credentials'
	}}}
   stage('build') {
            steps {
		sh "cd jenkins-lab-2-packer && make init && make stop && make start"
            	sh "docker exec -it packerDemo bash make build"
              
              
            }
     }
}
}
