pipeline {

    agent any

    tools {
        maven 'M2_HOME'        // Your Maven tool name
        // jdk 'JAVA_HOME'        // Your JDK tool name
    }

    stages {

        stage('Code Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/SaidiAziz/student-management.git'
            }
        }

        stage('Code Build') {
            steps {
                sh 'mvn clean install -DskipTests=true'
            }
        }

        // stage('Test Reports') {
        //     steps {
        //         junit '**/target/surefire-reports/*.xml'
        //     }
        // }

        // stage('Archive JAR') {
        //     steps {
        //         archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        //     }
        // }
    }

    post {
        always {
            echo "======always======"
            cleanWs()
        }

        success {
            echo 'Build completed successfully!'
        }

        unstable {
            echo 'Build is unstable.'
        }

        failure {
            echo 'Build failed!'
        }
    }
}
