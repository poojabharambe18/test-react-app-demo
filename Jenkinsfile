pipeline {
    agent {
        label 'jenkins-agent1' // ✅ Updated agent label
    }

    environment {
        NODE_HOME = '/usr/bin/node' // ✅ Your Node.js path
        PATH = "$NODE_HOME/bin:$PATH"
    }

    stages {

        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'master', 
                    credentialsId: 'github-react-credentials', 
                    url: 'https://github.com/poojabharambe18/test-react-app-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Code') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }

        // OPTIONAL: For local serving/testing, not for production
        stage('Serve React App (Optional)') {
            steps {
                sh 'npm install -g serve'
                sh 'serve -s build -l 5000 &'
            }
        }
    }
}

