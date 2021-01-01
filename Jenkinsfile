pipeline {
    agent {
        docker {
            image 'hernanku/jenkin-node-agent:latest'
        }
    }
    parameters {
        string (defaultValue: '1.0', description: 'Please enter release version number', name: 'RELEASE')
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "192.168.1.127:8081"
        NEXUS_CREDENTIAL_ID = "nexus-creds"
        BuildNumber = "${env.BUILD_NUMBER}"
        Release = "${params.RELEASE}"
        BuildVersion = "${params.RELEASE}.${env.BUILD_NUMBER}"
        appName = "ui-content-app"
    }
    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'feature/jenkins-cicd',
                    credentialsId: 'jenkins-ssh',
                    url: "https://github.com/hernanku/${appName}.git"
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
                zip -r $appName.$BuildVersion.zip .
                ls -altr
                '''
            }
        }

        stage('Artifactory Upload') {
            steps {
                echo "${env.NEXUS_URL}"
                echo "${env.NEXUS_REPOSITORY}"
                echo "${env.NEXUS_CREDENTIAL_ID}"
                script {
                    nexusArtifactUploader(
                        nexusVersion: NEXUS_VERSION,
                        protocol: NEXUS_PROTOCOL,
                        nexusUrl: NEXUS_URL,
                        repository: "${appName}",
                        credentialsId: NEXUS_CREDENTIAL_ID,
                        artifacts: [
                            [
                                artifactId: "${appName}",
                                classifier: '',
                                file: "${appName}.${BuildVersion}.zip",
                                type: 'zip'
                            ]
                        ]
                    )
                }
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
