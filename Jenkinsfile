pipeline {
    agent any

    // Install the Jenkins tools you need for your project / environment
    tools {nodejs "nodejs"}

    // Pull your Snyk token from a Jenkins encrypted credential
    // (type "Secret text"... see https://jenkins.io/doc/book/using/using-credentials/#adding-new-global-credentials)
    // and put it in temporary environment variable for the Snyk CLI to consume.
    environment {
        SNYK_TOKEN = credentials('Alexandra_Snyktoken')
    }

    stages {

        stage('Initialize & Cleanup Workspace') {
            steps {
               println 'Initialize & Cleanup Workspace'
               cleanWs()
            }
        }

        stage('Git Clone') {
            steps {
                git url: 'https://github.com/ACsnyk/snyk-goof.git'
                sh 'ls -la'
            }
        }

        stage('Test Build Requirements') {
            steps {
                sh 'npm -v'
                sh 'node -v'
            }
        }

        // Not required if just install the Snyk CLI on your Agent
        stage('Download Snyk CLI') {
            steps {
                script{
                latest_version=sh(script: """
                curl -Ls "https://github.com/snyk/snyk/releases/latest" | grep "^location" |awk -F'/' '{print \$NF}'
                """, returnStdout: true)
                //curl -L -Is "https://github.com/snyk/snyk/releases/latest" | grep "^location" | sed s#.*tag/##g | tr -d "\r"
                println "[INFO] Extracted latest version: ${latest_version}"
                // sh '''
                //     echo "Latest Snyk CLI Version: ${latest_version}"

                //     snyk_cli_dl_linux="https://github.com/snyk/snyk/releases/download/${latest_version}/snyk-linux"
                //     echo "Download URL: ${snyk_cli_dl_linux}"

                //     curl -Lo ./snyk "${snyk_cli_dl_linux}"
                //     chmod +x snyk
                //     ls -la
                //     ./snyk -v
                // '''
                }
            }
        }

        // // Run snyk test to check for vulnerabilities and fail the build if any are found
        // // Consider using --severity-threshold=<low|medium|high> for more granularity (see snyk help for more info).
        // stage('Snyk Test using Snyk CLI') {
        //     steps {
        //         sh './snyk test'
        //     }
        // }

        // // Capture the dependency tree for ongoing monitoring in Snyk.
        // // This is typically done after deployment to some environment (ex staging, test, production, etc).
        // stage('Snyk Monitor using Snyk CLI') {
        //     steps {
        //         // Use your own Snyk Organization with --org=<your-org>
        //         sh './snyk monitor --org=alexandra.catana'
        //     }
        // }
    }
}