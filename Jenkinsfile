pipeline {
    agent  { label 'jenkins-slave'}
    
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
                sh 'docker build -t localhost:5000/gatsby-admin-template:latest .'
            }
        }
        stage('docker push') {
            steps {
                sh 'docker push localhost:5000/gatsby-admin-template:latest'
            }
        }
        stage('kube deploy') {
            steps {
                sh 'kubectl delete -n react-projects -f deployment.yaml && kubectl apply -n react-projects -f deployment.yaml'
            }
        }
    }
} 