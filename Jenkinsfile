pipeline {
    agent any

    stages {
        stage('Collect artifacts') {
            steps {
                script {
                    step ([$class: 'CopyArtifact',
                          projectName: 'test_build',
                          filter: "provider-1*.json",
                          optional: true,
                          selector: lastSuccessful(),
                          target: 'all_json']);

                    step ([$class: 'CopyArtifact',
                          projectName: 'test_build',
                          filter: "provider-2*.json",
                          optional: true,
                          selector: lastSuccessful(),
                          target: 'all_json']);

                    sh 'tar -czf all_json.tgz all_json/'
                    archiveArtifacts 'all_json.tgz'
                }
            }
        }
    }
}
