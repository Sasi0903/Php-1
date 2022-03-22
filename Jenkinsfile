pipeline{
    agent any
    environment{
        IMAGE_NAME ='sas0506/java-mvn-privaterepo:php$BUILD_NUMBER'
        SERVER_IP ='ec2-user@172.31.43.80'
    }
    stages{
        stage("Build Docker image"){
            steps{
                script{
                    sshagent(['Test_Server-key']) {
            withCredentials([usernamePassword(credentialsId: 'HubID', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
            echo "Building the docker image"
         sh "scp -o StrictHostkeyChecking=no -r docker-files ${SERVER_IP}:/home/ec2-user"
         sh "ssh -o StrictHostkeyChecking=no ${SERVER_IP} 'bash ~/docker-files/docker-script.sh'"
         sh "ssh -o StrictHostkeyChecking=no ec2-user@172.31.43.80 sudo docker build -t ${IMAGE_NAME} /home/ec2-user/docker-files/dockerfile" 
         sh "ssh ${SERVER_IP} sudo docker login -u $USERNAME -p $PASSWORD"
         sh "ssh ${SERVER_IP} sudo docker push ${IMAGE_NAME}"  
         sh "ssh ${SERVER_IP} sudo docker-compose -f docker-compose.yml up -d"                      
            }   
        }
                                    }
    }
}
    }
}