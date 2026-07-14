pipeline {
    agent any


    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/sivareddy78936/Final-deployment-.git'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                dir('Backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                dir('Frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('Frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Build Completed') {
            steps {
                echo 'Build completed successfully.'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
