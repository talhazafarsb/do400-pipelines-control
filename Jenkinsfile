pipeline {
    agent {
        node {
            label 'nodejs'
        }
    }
     parameters {
	booleanParam(name: "RUN_FRONTEND_TESTS", defaultValue: true)
     }
    stages {
        stage('Checkout') {
            steps { // Missing "steps" block added here
                git branch: 'main',
                    url: 'https://github.com/talhazafarsb/do400-pipelines-control'
            }
        }
	stage('Run Tests') {
            parallel {
                stage('Backend Tests') {
                    steps {
                        sh 'node ./backend/test.js'
                    }
                }
                stage('Frontend Tests') {
		    when { expression { params.RUN_FRONTEND_TESTS } }
                    steps {
                        sh 'node ./frontend/test.js'
                    }
                }
            }
        }
	stage('Deploy') {
            when {
                expression { env.GIT_BRANCH == 'origin/main' }
            }
            steps {
                echo 'Deploying...'
            }
        }
    }
}
