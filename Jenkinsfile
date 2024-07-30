pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
                // Checkout code from GitHub and specify the branch
                git branch: 'main', url: 'https://github.com/Ayrola/JenkinsSeleniumWebDriverRepo'
            }
        }

        stage('Set up .NET Core') {
            steps {
                bat '''
                echo Downloading .NET SDK 6.0 installer
                curl -L -o dotnet-sdk-6.0.132-win-x86.exe https://download.visualstudio.microsoft.com/download/pr/ad59f1d1-5f19-4474-86be-2f09ab195618/5c7a64445dae84e386bb88e1f6ac09e4/dotnet-sdk-6.0.132-win-x86.exe
                echo Installing .NET SDK 6.0
                dotnet-sdk-6.0.132-win-x86.exe /quiet /norestart
                '''
            }
        }

         stage('Restore dependencies') {
            steps {
                // Restore dependencies using the solution file
                bat 'dotnet restore SeleniumBasicExercise.sln'
            }
        }


        stage('Build') {
            steps {
                // Build the project using the solution file
                bat 'dotnet build SeleniumBasicExercise.sln --no-restore'
            }
        }

        stage('Run TestProject1 tests') {
            steps {
                bat '''
                echo "Running TestProject1 tests"
                dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"
                '''
            }
        }

        stage('Run TestProject2 tests') {
            steps {
                bat '''
                echo "Running TestProject2 tests"
                dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"
                '''
            }
        }

        stage('Run TestProject3 tests') {
            steps {
                bat '''
                echo "Running TestProject3 tests"
                dotnet test TestProject3/TestProject3.csproj --logger "trx;LogFileName=TestResults.trx"
                '''
            }
        }
    }

    post {
        always {
            // Archive the TRX test result files
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            // Convert and publish the TRX results as JUnit results using the MSTest plugin
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}