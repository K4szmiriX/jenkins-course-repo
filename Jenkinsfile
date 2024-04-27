pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven-3"
        jdk "jdk-17"
    }


    stages {
        stage('Checkout') {
            git url: 'https://github.com/K4szmiriX/jenkins-course-repo.git', branch: 'master'
        }

        stage('Build') {
            when {
                expression {
                    !currentBuild.changeSets.isEmpty() && !currentBuild.changeSets[0].msg.contains('[ci skip]')
                }
            }
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean install"
            }
        }
    }

    post {
        // If Maven was able to run the tests, even if some of the test
        // failed, record the test results and archive the jar file.
        success {
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
            slackSend color: "good", message: "Message from Jenkins Pipeline"
        }
    }
}
