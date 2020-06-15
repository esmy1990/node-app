pipeline
{
 agent any
	environment
	{
  	def mvnhome = tool name: 'maven 3.6.3', type: 'maven'
	def mvncmd = "${mvnhome}/bin/mvn"
	NAME = "my-app"
    VERSION = "${env.BUILD_ID}"
    IMAGE = "${NAME}:${VERSION}"
	}
 stages
{
	stage("checkout") 
	{
		steps
		{
		git 'https://github.com/esmy1990/my-app'
		}
	}
	
	stage("maven build")
	{
		steps
		{
			sh  "${mvncmd} clean package"
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
}}
