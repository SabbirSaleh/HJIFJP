pipeline {
    agent any
    environment {
        FABRIC_BIN = '/path/to/fabric/bin'  // Update with actual path
        IMAGE_NAME = 'myapp'
        IMAGE_TAG = 'v1.0'
        CONTAINER_NAME = 'myapp-container'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install' // Assuming the build tool is Maven; change if it's something else
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .' // Builds the Docker image for the application
            }
        }
        stage('OWASP Dependency Check') {
            steps {
                sh 'dependency-check.sh --project "MyProject" --scan . --format "ALL"' // Performs dependency vulnerability checks
            }
        }
        stage('Deploy Fabric Network') {
            steps {
                sh './network.sh up createChannel -c mychannel -ca' // Starts Hyperledger Fabric Network
                sh './network.sh deployCC -c mychannel -ccn mycc -ccp ../chaincode/chaincode-java -ccl java' // Deploys chaincode to the Fabric Network
            }
        }
        stage('Deployment') {
            steps {
                sh 'docker run -d --name $CONTAINER_NAME -p 80:80 $IMAGE_NAME:$IMAGE_TAG' // Deploys the application in a container
            }
        }
    }
}
