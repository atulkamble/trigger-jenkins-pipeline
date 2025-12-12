pipeline {
    agent any

    triggers {
        cron('H/2 * * * *') // Runs at 2 AM every weekday (Monday to Friday)
    }
        
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
