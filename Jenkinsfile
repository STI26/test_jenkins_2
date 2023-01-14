pipeline {
    agent any

    stages {
        stage('Collect artifacts') {
            steps {
                script {
                    buildNumber = Jenkins.instance.getItem('test_build').lastSuccessfulBuild.number

                    for(int i = buildNumber; i >= 0; i--) {
                        try {
                            copyArtifacts(
                                projectName: 'test_build',
                                filter: "provider-1*.json",
                                selector: specific("${i}"),
                                target: 'all_json'
                            );
                            break;
                        } catch {
                            echo "provider-1* not found in build #${i}"
                        }
                    }
                    
                    for(int i = buildNumber; i >= 0; i--) {
                        try {
                            copyArtifacts(
                                projectName: 'test_build',
                                filter: "provider-2*.json",
                                selector: specific("${i}"),
                                target: 'all_json'
                            );
                            break;
                        } catch {
                            echo "provider-2* not found in build #${i}"
                        }
                    }

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
