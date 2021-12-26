node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/dsrdsr8/ds-ja-wa-ap-doc.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      
      sh 'mvn clean install'
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t dsrdsr8/java-web-app-ds .'
    }
    
    stage('Push Docker Image'){
        withCredentials([usernamePassword(credentialsId: 'uidpwd_method', passwordVariable: 'githubpassword', usernameVariable: 'githubuser')]) {
          sh "docker login -u ${githubuser} -p ${githubpassword}"
        }
        sh 'docker push dsrdsr8/java-web-app-ds'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app-ds dsrdsr8/java-web-app-ds'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.20.72 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.20.72 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.20.72 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.20.72 ${dockerRun}"
       }
       
    }
	}
     
     
}
