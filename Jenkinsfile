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
                echo "I am here" 
                aws ecr get-login-password --region '$AWS_DEFAULT_REGION' | docker login --username AWS --password-stdin '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com
                """, returnStdout: true) 
            }
        }

        stage('git clone') {
            steps{
                sh(script: """
                echo "I am here2"
                rm -rf Edureka_Mid_Project
                echo "I am here3"
                git clone https://github.com/thesnehasish/Edureka_Mid_Project.git
                echo "I am here4"
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
                #docker push aimvector/python:${BUILD_NUMBER}
                docker tag productapi:latest '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com/productapi:latest
                docker tag administrator:latest '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com/administratorapi:latest
                docker tag demand:latest '$AWS_ACCOUNT_ID'.dkr.ecr.'$AWS_DEFAULT_REGION'.amazonaws.com/demandapi:latest
                
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
                cd $WORKSPACE/Edureka_Mid_Project//
                #get kubectl for this demo
                curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
                chmod +x ./kubectl
                
                #./kubectl apply -f ./kubernetes/configmaps/configmap.yaml
                #./kubectl apply -f ./kubernetes/secrets/secret.yaml
                #cat ./kubernetes/deployments/deployment.yaml | sed s/1.0.0/${BUILD_NUMBER}/g | ./kubectl apply -f -
                #./kubectl apply -f ./kubernetes/services/service.yaml               
                
                ./kubectl apply -f ./backend/deploy-product.yml
                ./kubectl apply -f ./backend/deploy-demand.yml
                ./kubectl apply -f ./backend/deploy-administrator.yml
                '''
                */
        }
    }
}
}
