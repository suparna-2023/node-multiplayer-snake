node ('ubuntu'){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  

    stage('SAST') {
        build 'SCA-SAST-SNYK'
    }
    
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("suparna2023/snake")
    }
    stage('Post-to-dockerhub') {
    
     docker.withRegistry('https://registry.hub.docker.com', 'training_creds') {
            app.push("latest")
        			}
         }

    stage ('DAST') {
        build 'SECURITY-DAST-OWASP_ZAP'
    }
    
  
    
    stage('Pull-image-server') {
    
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
 
}

