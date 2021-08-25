node {

    def buildNumber = BUILD_NUMBER
    stage("Git CheckOut"){
        git url: 'https://github.com/gitniran/java-web-app-docker.git',branch: 'master'
    }

    stage("Maven Clean Package"){
        def mavenHome =  tool name: "Maven", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    
    stage("Build Dokcer Image") {
        sh "docker build -t niranjan2892/javawebapp:${buildNumber} ."
    }
    
    stage("Docker login and Push"){
        withCredentials([string(credentialsId: 'docker_hub_password', variable: 'docker_hub_password')]) {
            sh "docker login -u niranjan2892 -p ${docker_hub_password} " 
    }
        sh "docker push niranjan2892/javawebapp:${buildNumber}"
    }
    stage("Deploy to dockercontinor in docker deployer"){
        sshagent(['docker_ssh_password']) {
            sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.188 docker rm -f cloudcandy || true"
            sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.188 docker run -d -p 8080:8080 --name cloudcandy niranjan2892/javawebapp:${buildNumber}"           
    }
    }
}
