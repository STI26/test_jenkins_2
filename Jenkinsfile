pipeline {
    agent any

    stages {
        stage('Collect artifacts') {
            steps {
                script {
                    buildNumber = Jenkins.instance.getItem('jobName').lastSuccessfulBuild.number

                    for(int i = buildNumber; i >= 0; i--) {
                        res = copyArtifacts(
                            projectName: 'test_build',
                            filter: "provider-1*.json",
                            selector: specific("${i}"),
                            optional: true,
                            target: 'all_json']
                        );
                        echo "result: ${res}"
                    }

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
    
    post {
        always {
            script {
                cleanWs()
            }
        }
    }
}
