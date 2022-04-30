pipeline {
    agent any
    
    stages {
        stage('test1'){
            steps {
                echo "HELLO ALL1"
            }
        }
        
        stage('test2'){
            input {
                message "What is your name?"
                ok "Next"
                parameters {
                    string(name: 'NAME', defaultValue: 'GILDONG', description: 'type your name')
                }
            }
            
            steps {
                echo "HELLO ${NAME}"
            }
        }
        stage('test3'){
            input {
                message "Docker image tag name"
                ok "finished"
                parameters {
                    string(name: 'TAG', defaultValue: 'none', description: 'type docker image tag')
                }
            }
            steps{
                sh'''
                sudo rm -rf /var/lib/jenkins/workspace/testgit/testgit
                git clone https://github.com/tnsdyd/testgit.git
                cd /var/lib/jenkins/workspace/testgit/testgit
                sudo docker build -t mm0820/testgit:${TAG} .
                sudo docker push mm0820/testgit:${TAG}
                sudo sed -i 's@image: nginx@image: mm0820/testgit:'"${TAG}"'@g' 1234.yaml
                sudo kubectl apply -f 1234.yaml
                sudo kubectl get deploy,svc
                '''
            }
        }
        stage('test4'){
            input {
                message "image2"
                ok "Next2"
                parameters{
                    string(name: 'TAG2', defaultValue: 'test4', description: 'type docker image tag')
                }
            }
            steps{
                sh'''
                cd /var/lib/jenkins/workspace/testgit/testgit
                echo "@@@@@" > index.html
                sudo docker build -t mm0820/testgit:${TAG2} .
                sudo docker push mm0820/testgit:${TAG2}
                sudo kubectl set image deploy deploy-nginx nginx-ctn=mm0820/testgit:${TAG2}
                '''
            }
        }
    }
        post {
        always {
            echo "작업완료 - 성공실패여부 모름"
            }
    }
}


