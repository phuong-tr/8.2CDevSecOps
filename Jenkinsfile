pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = 'phuong-tr_8.2CDevSecOps'
        SONAR_ORGANIZATION = 'phuong-tr'
        SONAR_TOKEN = credentials('sonarcloud-token')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/phuong-tr/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh """
                    npx sonar-scanner \
                      -Dsonar.projectKey=${env.SONAR_PROJECT_KEY} \
                      -Dsonar.organization=${env.SONAR_ORGANIZATION} \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=${env.SONAR_TOKEN}
                    """
                }
            }
        }
    }
}
