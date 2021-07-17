node{
   stage('SCM Checkout'){
     git 'https://github.com/JishG/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3.6', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   
   stage('Build Docker Imager'){
     
     sh 'docker build -t jishg/myweb .'
   }
   stage('Docker Image Push'){
   
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
    sh "docker login -u jishg -p ${dockerPassword}"
   }
   sh 'docker push jishg/myweb'
   } 
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 15.207.89.63:8083"
   sh "docker tag jishg/myweb 15.207.89.63:8083/jishg/myweb"
   sh 'docker push 15.207.89.63:8083/jishg/myweb'
   }
     stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
	}
   stage('Docker deployment'){
   
   sh 'docker run -d -p 8090:8080 --name tomcattest jishg/myweb'
   }
}
