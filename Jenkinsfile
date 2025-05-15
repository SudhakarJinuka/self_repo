pipeline {
    agent any

    parameters {
        // 1. Boolean Parameter
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run unit tests after build?')

        // 2. Choice Parameter
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'prod'], description: 'Choose deployment environment')

        // 3. Credentials Parameter (Username/Password)
        credentials(name: 'DEPLOY_CREDS', credentialType: 'com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl', description: 'Deployment credentials')

        // 4. File Parameter
        file(name: 'CONFIG_FILE', description: 'Upload your configuration file')

        // 5. Multi-line String Parameter
        text(name: 'CUSTOM_SCRIPT', defaultValue: 'echo "Custom deployment script..."', description: 'Enter multi-line script')

        // 6. Password Parameter
        password(name: 'API_SECRET', defaultValue: '', description: 'Enter your secret API token')

        // 7. Run Parameter
        run(name: 'SOURCE_BUILD', projectName: 'upstream-job', description: 'Select build from upstream-job')

        // 8. String Parameter
        string(name: 'BRANCH', defaultValue: 'main', description: 'Enter Git branch name')
    }

    stages {
        stage('Print Parameters') {
            steps {
                echo "Run Tests: ${params.RUN_TESTS}"
                echo "Environment: ${params.ENVIRONMENT}"
                echo "Branch: ${params.BRANCH}"
                echo "Selected Build from Upstream Job: ${params.SOURCE_BUILD}"
            }
        }


        stage('Use Password Parameter') {
            steps {
                sh "echo 'Using API Token: ${params.API_SECRET}'" // Note: masked in console if printed carefully
            }
        }

        stage('Upload File') {
            steps {
                sh 'echo "Contents of uploaded file:"'
                sh 'cat $CONFIG_FILE'
            }
        }

        stage('Run Custom Script') {
            steps {
                sh "${params.CUSTOM_SCRIPT}"
            }
        }

        stage('Run Tests') {
            when {
                expression { return params.RUN_TESTS == true }
            }
            steps {
                echo "Running unit tests..."
                sh 'echo "Tests executed"'
            }
        }

        stage('Use Run Parameter') {
            steps {
                echo "Triggering or using build artifacts from build: ${params.SOURCE_BUILD}"
                // Example: Copy artifacts from the selected build
                copyArtifacts(projectName: 'upstream-job', selector: specific("${params.SOURCE_BUILD}"))
            }
        }
    }

    post {
        always {
            echo "Pipeline script execution completed sucessfully."
        }
    }
}
