pipeline {
    agent { label 'windows-java21-agent' }
    options {
        timestamps()
        disableConcurrentBuilds()
        skipDefaultCheckout()
        buildDiscarder(logRotator(numToKeepStr: '20'))
    }
    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Toolchain') { steps { bat 'java -version'; bat 'mvn -version'; bat 'git --version' } }
        stage('Build and Verify') { steps { bat 'mvn -B -ntp clean verify' } }
        stage('Generate Reports') { steps { bat 'mvn -B -ntp surefire-report:report-only'; bat 'mvn -B -ntp jacoco:report' } }
    }
    post {
        always {
            junit allowEmptyResults: true, testResults: 'target/surefire-reports/TEST-*.xml,target/failsafe-reports/TEST-*.xml'
            archiveArtifacts artifacts: 'target/surefire-reports/**,target/failsafe-reports/**,target/site/**', allowEmptyArchive: true
        }
        success { archiveArtifacts artifacts: 'target/task-manager-cicd-1.0.0.jar', fingerprint: true }
        failure { echo 'Pipeline failed. Check Console Output and reports.' }
        unstable { echo 'Pipeline is unstable. Check tests.' }
    }
}
