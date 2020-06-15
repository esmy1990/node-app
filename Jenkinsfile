pipeline
{
 agent any
	environment
	{
  	def mvnhome = tool name: 'maven 3.6.3', type: 'maven'
	def mvncmd = "${mvnhome}/bin/mvn"
	NAME = "node-app"
    VERSION = "${env.BUILD_ID}"
    IMAGE = "${NAME}:${VERSION}"
	}
 stages
{
	stage("checkout") 
	{
		steps
		{
		git 'https://github.com/esmy1990/node-app'
		}
	}
	
	
	stage ("docker build")
	{
		steps
		{
			sh "docker build -t esmy1990/${IMAGE} ."
		}
	}
	stage ("docker push")
	{
		
		steps
		{
			withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerpwd')]) 
			{
		
			sh "docker login -u esmy1990 -p ${dockerpwd}"
			}
		
			sh "docker  push esmy1990/${IMAGE}"
		}
	}
	stage ("deploy to k8s" )
	{
		steps{
		sh "chmod +x changeTag.sh" 
			sh "./changeTag.sh ${IMAGE}"
		sshagent (credentials: ['dev']) {
    sh 'ssh -o StrictHostKeyChecking=no root@192.168.0.7 uptime'
            sh 'ssh -v root@192.168.0.7'
			sh 'scp services.yml root@192.168.0.7:/home'
  }
		}
	}
}}
