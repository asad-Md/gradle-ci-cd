# Procedure

- gradle init --type java-application --dsl groovy --test-framework junit-jupiter

- docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts

- docker run -d -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home --name my-jenkins jenkins/jenkins:lts
- or   docker start my-jenkins
- [http://localhost:8080](http://localhost:8080)
- f69c9c634c93416e9627f118172b63d9

- To run Build File: ./gradlew :app:run --console plain
- OR java -jar app/build/libs/app.jar

- pipeline script:
pipeline {
    agent any

    stages {
        stage('Cleanup') {
            steps {
                // Ensures a fresh start every time
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                // Using your specific GitHub URL and 'main' branch
                git branch: 'main', url: 'https://github.com/asad-Md/gradle-ci-cd.git'
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x gradlew'
                // This builds the sub-project specifically
                sh './gradlew :app:build' 
            }
        }
        stage('Test') {
            steps {
                // Running the tests for your To-Do App
                sh './gradlew test'
            }
        }
        stage('Archive Artifacts') {
            steps {
                // Point to the actual location of the jar
                archiveArtifacts artifacts: 'app/build/libs/*.jar', fingerprint: true
            }
        }
        
    }

    post {
        success {
            echo 'Build and Testing completed successfully!'
        }
        failure {
            echo 'The build failed. Check the console output above for errors.'
        }
    }
}

- 