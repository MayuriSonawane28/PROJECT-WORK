node {
    stage('git checkout') {
        git 'https://github.com/MayuriSonawane28/PROJECT-WORK.git'
    }

    stage('build and packaging') {
        sh 'mvn clean package'
    }

    stage('building docker image') {
        sh 'docker build -t mayuri/project:v1 .'
    }

    stage('push to dockerhub') {
        withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerpass')]) {
            sh "docker login -u mayuri -p ${dockerpass}"
            sh 'docker push mayuri/project:v1'
        }
    }

    stage('deploy on ansible') {
        ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', 
inventory: '/etc/ansible/hosts', playbook: '/var/lib/jenkins/workspace/Project-Work/playbook.yml', vaultTmpPath: ''
    }
}
