node{
   stage('SCM Checkout'){
     git 'https://github.com/vignesh100198/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t vignesh1001/myweb:0.0.1 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u vignesh1001 -p ${dockerPassword}"
    }
   sh 'docker push vignesh1001/myweb:0.0.1'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p viki123 3.90.225.158:9090"
   sh "docker tag vignesh1001/myweb:0.0.1 3.90.225.158:9090/viki:1.0"
   sh 'docker push 3.90.225.158:9090/viki:1.0'
   }

   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest vignesh1001/myweb:0.0.1'
   }
}
