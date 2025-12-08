pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'     // Configure this JDK in Jenkins global tools
        maven 'M2_HOME'     // Configure this Maven in Jenkins global tools
    }

    options {
        timestamps()
        skipDefaultCheckout()
    }

    environment {
        APP_NAME      = 'student-management'
        IMAGE_NAME    = "student-management:${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn -B clean verify'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }

        stage('Docker Build') {
            when {
                expression { fileExists('Dockerfile') }
            }
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo "Build completed successfully for ${APP_NAME}"
        }
        unstable {
            echo 'Build is unstable.'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
