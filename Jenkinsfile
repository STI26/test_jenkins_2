SERVER_OPTIONS = [
    'develop': [ host_server: '123' ],
    'Select':[],
]

pipeline {
    agent any
    parameters {
        choice(
            name: 'SERVER',
            choices: SERVER_OPTIONS.keySet() as List,
            description: 'Select server',
        )
    }
    stages {
        stage('Collect artifacts') {
            steps {
                script {
                    buildNumber = Jenkins.instance.getItem('test_build').lastSuccessfulBuild.number

                    copyLatestArtifact('provider-1', 'test_build', buildNumber, 'all_json')
                    
                    copyLatestArtifact('provider-2', 'test_build', buildNumber, 'all_json')

                    sh 'tar -czf all_json.tgz all_json/'
                    archiveArtifacts 'all_json.tgz'

                    app = 'provider-2'
                    fileName = sh(
                        script: "find lib/build/ -name ${app}*.json",
                        returnStdout: true
                    ).trim()
                    sh 'ls'
                    sh 'pwd'

                    archiveArtifacts fileName
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

def copyLatestArtifact(String providerName, String projectName, Integer lastBuildNumber, String target) {
    for(int i = lastBuildNumber; i >= 0; i--) {
        try {
            copyArtifacts(
                projectName: projectName,
                filter: "${providerName}*.json",
                selector: specific("${i}"),
                target: target
            );
            break;
        } catch(Exception e) {
            echo "${providerName} not found in build #${i}"
        }
    }
}
