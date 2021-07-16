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
                script {
                def NexusRepo = Version.endsWith("SNAPSHOT") ? "SankarDevOpsLab-SNAPSHOT" : "SankarDevOpsLab-RELEASE"
                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: '79309781-b415-4144-b8b9-40eae76cdbb7', 
                groupId: "${GroupId}", 
                nexusUrl: '172.20.10.212:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
                }
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

        // Stage5 : Deploying build artifact on Tomcat server
        stage ('Deploy on Tomcat Server') {
            steps {
                echo 'Deploying......'
                sshPublisher(publishers: 
                [sshPublisherDesc(configName: 'Ansible_Controller', 
                transfers: [sshTransfer(cleanRemote: false, excludes: '', 
                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts', execTimeout: 120000, flatten: false, 
                makeEmptyDirs: false, 
                noDefaultExcludes: false, 
                patternSeparator: '[, ]+', 
                remoteDirectory: '', 
                remoteDirectorySDF: false, 
                removePrefix: '', 
                sourceFiles: '')], 
                usePromotionTimestamp: false, 
                useWorkspaceInPromotion: false, 
                verbose: false)])
            }
        }
     
       // Stage6 : Deploying the build artifact to Docker
        stage ('Deploy on Docker') {
            steps {
                echo 'Deploying......'
                sshPublisher(publishers: 
                [sshPublisherDesc(configName: 'Ansible_Controller', 
                transfers: [sshTransfer(cleanRemote: false, excludes: '', 
                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts', 
                execTimeout: 120000)], 
                usePromotionTimestamp: false, 
                useWorkspaceInPromotion: false, 
                verbose: false)])
            }
        }
    }
}
