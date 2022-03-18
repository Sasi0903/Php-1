pipeline{
    agent any
    stages{
        stage("Build Docker image"){
            steps{
                script{
                    sshagent(['Test_Server-key']) {
            withCredentials([usernamePassword(credentialsId: 'HubID', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
            echo "Building the docker image"
         sh "scp -o StrictHostkeyChecking=no docker-script.sh ec2-user@172.31.43.80:/home/ec2-user"
         sh "ssh -o StrictHostkeyChecking=no ec2-user@172.31.43.80 'bash ~/docker-script.sh'"
         sh "ssh ec2-user@172.31.43.80 sudo docker build -t newimage:php$BUILD_NUMBER ." 
         sh "ssh ec2-user@172.31.43.80 sudo docker login -u $USERNAME -p $PASSWORD"
         sh "ssh ec2-user@172.31.43.80 sudo docker push sas0506/java-mvn-privaterepo:php$BUILD_NUMBER"  
         sh "ssh ec2-user@172.31.43.80 sudo docker-compose -f docker-compose.yml up -d"                      
            }   
        }
                                    }
    }
}
    }
}