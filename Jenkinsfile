pipeline {
		agent any 
    stages {
        stage('Git Pull') {
            steps {
				git url: 'https://github.com/jainpranay20/MiniProject.git',
				branch: 'main',
                credentialsId: 'github'
            }
        }
        stage('Maven Build and Test') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Docker build image') {
            steps {
                sh 'docker build -t jainpranay20/mini_project:latest .'
            }
        }
        stage('Publish Docker Images') {
            steps {
                withDockerRegistry([ credentialsId: "dockercred", url: "" ]) {
                    sh 'docker push jainpranay20/mini_project:latest'
                }
            }
        }
        
        stage('Ansible Deploy') {
             steps {
                  ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'playbook.yml' ,sudoUser: null
             }
        }
    }
    
    post {
        always {
            sh 'docker logout'
        }
    }
}
