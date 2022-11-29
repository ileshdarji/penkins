pipeline {
    agent {
        node { label 'cm-linux' }
    }
    parameters {
        string(name: 'PROJECT_NAME', defaultValue: '', description: 'myPEAP Test Case Automation')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Mark to run the tests')
        choice(name: 'TAGS', choices: ['logon', 'all'], description: 'Tests to run by tag')
    }
    environment {
        GIT_CRED = 'HSBCSharedLibs'
        GIT_BRANCH = 'cicd'
        GIT_URL = 'https://alm-github.systems.uk.hsbc/gfit-devops-automation/myPEAP-automation.git'
        GRID = 'true'
    }

    stages {
        stage ('SCM Checkout') {
            steps {
                git branch: "${GIT_BRANCH}", credentialsId: "${GIT_CRED}", url: "${GIT_URL}"
            }
        }
        stage ('Run BDD tests') {
            when {
                expression {params.RUN_TESTS}
            }
            steps {
                dir ('tests') {
                    sh """
                        set +x
                        python3 -m venv ../env
                        source ../env/bin/activate
                        cp /build_tools/terraform/adfs_venv/pip.conf ../env
                        pip install -r ../requirements.txt --trusted-host efx-nexus.systems.uk.hsbc --index-url https://efx-nexus.systems.uk.hsbc:8084/nexus/repository/pypi.proxy/simple
                        echo "### Running tests for project : ${PROJECT_NAME} ###"
                        python3 -m behave -t ${TAGS}
                        deactivate
                    """
                }
            }
        }
    }
    post {
        always {
            archiveArtifacts '**/behave.log, **/report.html'
            script {
                try {
                    junit 'tests/target/junit/*.xml'
                }
                catch(err) {
                    echo "Failed to parse junit report"
                }
            }
            echo 'Clean up the job workspace'
        }
    }
}
