pipeline {
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment {
       ArtifactId = readMavenPom().getArtifactId()
       Version = readMavenPom().getVersion()
       Name = readMavenPom().getName()
       GroupId = readMavenPom().getGroupId()
    }

    stages {
        // Specify various stage with in stages

        // stage1 : Build
        stage ('Build') {
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test') {
            steps {
                echo ' testing......'
            }
        }

        //stage3 : Publish the artifacts to Nexus
        stage ('Publish to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'SankarDevOpsLab', classifier: '', file: 'target/SankarDevOpsLab-0.0.1-SNAPSHOT.war', type: 'war']], credentialsId: 'ed2818dc-d8cc-4190-9557-ebe4134ad06d', groupId: 'com.sankardevopslab', nexusUrl: '172.20.10.54:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'SankarDevOpsLab-SNAPSHOT', version: '0.0.1-SNAPSHOT'
            }
        }

        // Stage4 : Print some Information
            stage ('Print Environment variables') {
                steps {
                    echo "Artifact ID is ${ArtifactId}"
                    echo "Version is ${Version}"
                    echo "Group ID is ${GroupId}"
                    echo "Name ID is ${Name}"
                }
            }

        // Stage5 : Deploying
        stage ('Deploy') {
            steps {
                echo 'Deployed Successfully......'
            }
        }
    }
}
