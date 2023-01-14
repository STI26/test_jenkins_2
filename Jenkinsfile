pipeline {
    agent any

    stages {
        stage('Collect artifacts') {
            steps {
                script {
                    step ([$class: 'CopyArtifact',
                          projectName: 'test_build',
                          filter: "provider-1*.json",
                          target: 'all_json']);

                    step ([$class: 'CopyArtifact',
                          projectName: 'test_build',
                          filter: "provider-2*.json",
                          target: 'all_json']);

                    archiveArtifacts 'all_json'
                }
            }
        }
    }
}
