pipeline {
   agent any
    environment{
        AWS_ACCESS_KEY_ID     ='AKIAZQGRZXSIBLWEWZE6'  
       //credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY ='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' 
       //credentials('jenkins-aws-secret-access-key')
        AWS_DEFAULT_REGION ='us-east-1' 
       //credentials('jenkins-aws-default-region')       
        AWS_ACCOUNT_ID= '653275020432' 
       //credentials('jenkins-aws-account-id')
    }
    stages {
        stage('docker login') {
            steps{
                sh(script: """                 
                aws ecr get-login-password --region '$AWS_DEFAULT_REGION' | docker login --username AWS --password-stdin '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com
                """, returnStdout: true) 
            }
        }

        stage('git clone') {
            steps{
                sh(script: """
                #Remove old files and clone new files
                rm -rf Edureka_Mid_Project                
                git clone https://github.com/thesnehasish/Edureka_Mid_Project.git               
                """, returnStdout: true) 
            }
        }

        stage('docker build') {
            steps{
               echo "docker build"
               echo "$AWS_DEFAULT_REGION"
                sh script: '''
                #!/bin/bash
                echo $WORKSPACE
                cd $WORKSPACE/Edureka_Mid_Project/backend
                docker-compose down --rmi "all"
                docker-compose up -d
                # docker build . --network host -t aimvector/python:${BUILD_NUMBER}                
                '''
            }
        }

        stage('docker push') {
            steps{
               echo "docker push"
                sh(script: """                
                docker tag productapi:latest '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com/productapi:latest
                docker tag administratorapi:latest '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com/administratorapi:latest
                docker tag demandapi:latest '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com/demandapi:latest
                
                docker push '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com/productapi:latest
                docker push '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com/administratorapi:latest
                docker push '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com/demandapi:latest
                """)
                
            }
        }

        stage('deploy') {
            steps{
               echo "deploy"
              /*  sh script: '''
                #!/bin/bash
                cd $WORKSPACE/Edureka_Mid_Project/
                #get kubectl for this demo
                curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
                chmod +x ./kubectl                
                
                #Deploy kubernates deployment file
                ./kubectl apply -f ./backend/deploy-product.yml
                ./kubectl apply -f ./backend/deploy-demand.yml
                ./kubectl apply -f ./backend/deploy-administrator.yml
                '''
                */
        }
    }
}
}
