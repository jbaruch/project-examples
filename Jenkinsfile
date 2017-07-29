pipeline {
    agent any

    stages {
        stage('Cleanup') {
          steps {
              script {
                if(CLEAN_REPO) {
                sh 'rm -rf ~/.m2/repository'
                }
             }
          }
        }
        stage('Build') {
            steps {
                script {
                    git url: 'https://github.com/jbaruch/project-examples'
                    def server = Artifactory.server ARTIFACTORY
                    def buildInfo = Artifactory.newBuildInfo()
                    buildInfo.env.capture = true
                    def rtMaven = Artifactory.newMavenBuild()
                    rtMaven.tool = MAVEN
                    rtMaven.resolver server: server, releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot'
                    rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'
                    rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean install', buildInfo: buildInfo
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    rtMaven.deployer.deployArtifacts buildInfo
                }
            } 
        }
    }
}
