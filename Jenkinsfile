pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/dariikhom/ITinfrastrLab4.git',
                    credentialsId: 'git-token'
            }
        }
        
        stage('Build') {
            steps {
                bat '"C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\MSBuild\\Current\\Bin\\MSBuild.exe" test_repos.sln /t:Build /p:Configuration=Debug'
            }
        }

        stage('Test') {
            steps {
                bat 'x64\\Debug\\test_repos.exe --gtest_output=xml:test_report.xml'
            }
        }
    }

    post {
    always {
            xunit(
                tools: [
                    GoogleTest(
                        pattern: 'test_report.xml',
                        skipNoTestFiles: false,
                        failIfNotNew: false,
                        deleteOutputFiles: true,
                        stopProcessingIfError: true
                    )
                ]
            )
        }
    }
}
