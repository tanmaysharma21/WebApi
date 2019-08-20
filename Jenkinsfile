pipeline {
    agent any
	
	parameters {
        string(defaultValue: "WebApplication10.sln", description: 'name of solution file', name: 'solutionName')
		string(defaultValue: "XUnitTestProject1/XUnitTestProject1.csproj", description: 'name of test file', name: 'testName')
		string(defaultValue: "api_image", description: 'name of docker image', name: 'docker_image_name')
		string(defaultValue: "tanmaysharma97/tanmay97", description: 'repository_name', name: 'repository_name')
		string(defaultValue: "01894222668", description: 'docker hub password', name: 'pass_word')
		string(defaultValue: "tanmaysharma97", description: 'docker hub username', name: 'user_name')
		string(defaultValue: "api_tag", description: 'tag name', name: 'tag_name')
    }
	
    stages { 
        stage('Build') {
        	
        	steps{
        		echo 'Build step'
        		bat 'dotnet build %solutionName% -p:Configuration=release -v:q'
        	}
        }
        stage('Test') {
        	
        	steps{
        		echo 'Test step'
        		bat 'dotnet test %testName%'
        	}
        }
        stage('Publish') {
        	
        	steps{
        		echo 'Publish step'
        		bat 'dotnet publish %solutionName% -c RELEASE -o Publish'
        	}
        }
		
		stage('Docker build Image') {
        	
        	steps{
        		echo 'Docker image'
        		bat 'docker build -t %docker_image_name% -f Dockerfile .'				
        	}
        }
		    stage('SonarQube stage') {
        	
        	steps{
        		echo 'Docker run the image pulled from dockerhub'
				bat 'dotnet C:/Users/tsharma/Documents/sonar-scanner/SonarScanner.MSBuild.dll begin /d:sonar.login=admin /d:sonar.password=admin /k:"f3c736460e86d1ce0e8748c22a06b38a800e89fe"'
				bat 'dotnet build'
				bat 'dotnet C:/Users/tsharma/Documents/sonar-scanner/SonarScanner.MSBuild.dll end /d:sonar.login=admin /d:sonar.password=admin'
        	}
        }
		
		stage('Docker hub Login') {
        	
        	steps{
        		echo 'Docker login to dockerhub'
				bat 'docker login -p %pass_word% -u tanmaysharma97'   		
        	}
        }
		stage('Docker push Image') {
        	
        	steps{
        		echo 'Docker push image to dockerhub'
				bat 'docker tag %docker_image_name% %repository_name%:%tag_name%'
				bat 'docker push %repository_name%:%tag_name%' 
				bat 'docker rmi %docker_image_name%:latest'
				bat 'docker rmi %repository_name%:%tag_name%'
        	}
        }
		
		stage('Docker pull Image') {
        	
        	steps{
        		echo 'Docker pull image from dockerhub'
				bat 'docker pull %repository_name%:%tag_name%'        		
        	}
        }
		
		stage('Docker run image') {
        	
        	steps{
        		echo 'Docker run the image pulled from dockerhub'
				bat 'docker run --rm -p 6789:6789 %repository_name%:%tag_name% '        		
        	}
        }
    }

}
///
