pipeline {
   agent none
   stages {
      stage('Source Code') {
         agent { label 'test' }
         steps {
            git 'https://github.com/venkys3/capstone2.git'
         }
      }
      stage('Build website') {
         agent { label 'test' }
         steps {
            sh "docker build -t mywebsiteapp:BUILD_NUMBER ."
			sh "docker run -d -p 82:80 --name=mywebsiteapp mywebsiteapp:BUILD_NUMBER"
         }
      }
	  stage('Website test') {
         agent { label 'test' }
         steps {
           sh "java -jar testcase.jar"
         }
	  }
	  stage('Push to Docker hub') {
         agent { label 'Build' }
         steps {
            sh "sudo docker push venkys3/mywebsiteapp:BUILD_NUMBER ."
         }
      }
	  stage('Publish to Production') {
         agent { label 'prod' }
         when {
          branch 'master'
          }
         steps {
            git 'https://github.com/venkys3/capstone2.git'
            sh "sudo docker stop mywebsiteapp 2> /dev/null || true"
            sh "sudo docker rm mywebsiteapp 2> /dev/null || true"
            sh "sudo docker run --name mywebsiteapp -itd -p 82:80 venkys3/mywebsiteapp:BUILD_NUMBER"
         }
	  }
	}
}
