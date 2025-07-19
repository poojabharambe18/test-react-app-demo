pipeline {
    agent {
        label 'jenkins-agent1' 
    }

    environment {
        NODE_HOME = '/usr/bin/node'
        PATH = "$NODE_HOME/bin:$PATH"

        SONAR_PROJECT_KEY = 'frontend-profile'
        SONAR_HOST_URL = 'http://10.14.1.49:9000'
        SONAR_TOKEN = 'sqp_05341675b56c3f5b904c2563b78b1886413dde93'
        SONARQUBE_SCANNER_HOME = tool 'sonarqube-token'  // Jenkins tool name
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

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube Scan...'
                withSonarQubeEnv('sonarqube-token') {
                    sh """
                        ${SONARQUBE_SCANNER_HOME}/bin/sonar-scanner \
                          -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=${SONAR_HOST_URL} \
                          -Dsonar.login=${SONAR_TOKEN} \
                          -Dsonar.sourceEncoding=UTF-8
                    """
                }
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

        stage('Serve React App') {
            steps {
                sh 'npm install -g serve'
                sh 'serve -s build -l 5000 &'
            }
        }
    }
}
