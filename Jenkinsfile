pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stages

        // stage1 : Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        //stage3 : Publish the artifacts to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'SankarDevOpsLab', classifier: '', file: 'target/SankarDevOpsLab-0.0.5-SNAPSHOT.war', type: 'war']], credentialsId: '03e30f0d-32ee-45ab-af3a-a04cd36ea0bc', groupId: 'com.sankardevopslab', nexusUrl: '172.20.10.54:8081', nexusVersion: 'nexus2', protocol: 'http', repository: 'SankarDevOpsLab-SNAPSHOT', version: '0.0.5-SNAPSHOT'
            }
        }

        // Stage4 : Deploying
        stage ('Deploy'){
            steps {
                echo 'Deployed Successfully......'
                }

            }
        }        
        
    }
