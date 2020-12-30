pipeline {
    agent {
        docker { 
            image 'hernanku/jenkin-node-agent:latest'
        }
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "http://localdev:8081"
        NEXUS_REPOSITORY = "ui-content-app"
        NEXUS_CREDENTIAL_ID = "nexus-creds"
    }
    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'feature/jenkins-cicd',
                    credentialsId: 'jenkins-ssh',
                    url: "https://github.com/hernanku/ui-content-app.git"
                // sh '''

                // '''
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

        stage ('Build') {
            steps {
                sh '''
                npm install
                pwd
                ls -altr
                zip -r ui-content-app.zip .
                ls -altr
                ''' 
            }
        }

        stage('Artifactory Upload') {
            steps {
                sh '''
                echo ${env.NEXUS_URL}
                echo ${env.NEXUS_REPOSITORY}
                echo ${env.NEXUS_CREDENTIAL_ID}
                '''
                // rtUpload (
                //     serverId: 'artifact-dev',
                //     specPath: 'artifact-upload.json'
                // )
            }
        }

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
            cleanWs()
        }
    }
}
