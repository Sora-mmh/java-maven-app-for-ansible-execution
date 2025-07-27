pipeline {   
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying all necessary files to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* ubuntu@13.61.19.134:/home/ubuntu"
                        // In case of scp on root user
                        // sh """
                        //     ssh -o StrictHostKeyChecking=no ubuntu@13.61.19.134 '
                        //     sudo mv /home/ubuntu/* /root/
                        // '
                        // """
			withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user)]) {
			  sh "scp ${keyfile} ubuntu@13.61.19.134:/home/ubuntu/ssh-key.pem"
			}
                    }
                }
            }
        }
    }
} 
