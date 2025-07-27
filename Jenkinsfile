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
                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh 'scp $keyfile ubuntu@13.61.19.134:/home/ubuntu/ssh-key.pem'
                        }
                    }
                }
            }
        }
        stage ("execute ansible playbook") {
            steps {
                script {
                    echo "calling ansible playbook to configure ec2 instances" 
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = "13.61.19.134"
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                        remote.user = user
                        remote.identityFile = keyfile
                        // sshCommand remote: remote, command: "cat /home/ubuntu/inventory_aws_ec2.yaml"
                        // sshCommand remote: remote, command: "ansible-playbook playbook.yaml"
                        sshCommand remote: remote, command: """
                            echo 'Checking AWS credentials...'
                            ls -la ~/.aws/ || echo 'No .aws directory'
                            env | grep AWS_ || echo 'No AWS env vars'
                        """
                    }
                
                }
            }
        }
    }
} 
