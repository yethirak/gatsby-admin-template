pipeline {
    agent  { label 'jenkins-slave'}
    environment {
        registry = 'localhost:5000'
        repo_name = 'gatsby-admin-template'
    }
    
    stages {
        stage('Checkout source') {
            steps {
                git branch: 'master', url: 'https://github.com/yethirak/gatsby-admin-template.git'
            }
        }
        stage('npm install and build') {
            steps {
                sh 'npm install && npm run build'
            }
        }
        stage('docker build') {
            steps {
                sh 'docker build -t $registry/$repo_name:$BUILD_NUMBER .'
            }
        }
        stage('docker push') {
            steps {
                sh 'docker push $registry/$repo_name:$BUILD_NUMBER'
            }
        }
        stage('docker remove unwanted image') {
            steps {
                sh 'docker rmi $registry/$repo_name:$BUILD_NUMBER'
            }
        }
        stage('kube deploy') {
            steps {
                sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -n react-projects -f -'
            }
        }
    }
} 
