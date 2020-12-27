pipeline {
    agent {
        docker { 
            image 'node:14-alpine' 
        }
    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'feature/jenkins-cicd', url: "git@github.com:hernanku/ui-content-app.git"
            }
        }
        // stage('Code Quality Check with SonarQube') {
        //     environment {
        //         scannerHome = tool 'sonar_scanner'
        //     }
        //     steps {
        //         withSonarQubeEnv('sonarServer') {
        //             sh "${scannerHome}/bin/sonar-scanner \
        //                 -Dsonar.projectKey=store-app-codeCheck \
        //                 -Dsonar.host.url=http://sonarqd01.trulabz.com:9000 \
        //                 -Dsonar.login=d6421646b431030bd7ea671b33d8a71db335da7f \
        //                 -Dsonar.sources=src/main/java/ \
        //                 -Dsonar.language=java \
        //                 -Dsonar.java.binaries=target/classes \
        //                 -Dsonar.java.libraries=target/*.jar"
        //         }
        //         // timeout(time: 2, unit: 'MINUTES') {
        //         //     waitForQualityGate abortPipeline: true
        //         // }
        //     }
        // }

        // stage ('Build') {
        //     steps {
        //         sh 'npm install' 
        //     }
        // }

        // stage('Artifactory Upload') {
        //     steps {
        //         rtUpload (
        //             serverId: 'artifact-dev',
        //             specPath: 'artifact-upload.json'
        //         )
        //     }
        // }

        // stage('Publish build info') {
        //     steps {
        //         rtPublishBuildInfo (
        //             serverId: 'artifact-dev'
        //         )
        //     }
        // }

        // stage ('Deploy to App Server') {
        //     steps{
        //         sh "sh pre-deploy.sh"
        //     }
        // }
    }
    post {
        always {
            sh "ls -ltr '${env.WORKSPACE}'"
            sh "ls -ltr '${env.WORKSPACE}/target'"
            deleteDir()
        }
    }
}

