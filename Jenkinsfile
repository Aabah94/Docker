node {    
    def app    
    stage('SCM checkout'){        
        git credentialsId: 'git-creds', url: 'https://github.com/jenkins540/myapp.git'
    }
    stage('MVN Package'){
        def mvnHome = tool name :'maven 3' , type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean package"
    }
    stage('Build Docker Package'){
        app= docker.build("aabha94/myapp:${env.BUILD_ID}")
    }
    stage('Deploy Image'){
        docker.app.withRun('--name myapp -p 8081:8080')
    }
    stage('Remove Container'){
        app.rm()
    }
    stage('Push Docker Image'){
        docker.withRegistry('https://registry.hub.docker.com', 'Docker-cred'){
        app.push()
        }
    }
}
