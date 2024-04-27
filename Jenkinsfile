pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven-3"
        jdk "jdk-17"
    }


    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/K4szmiriX/jenkins-course-repo.git', branch: 'master'
            }
        }

        stage('Build') {
//             when {
//                 expression {
//                     result = sh (script: "git log -1 | grep '.*\\[ci skip\\].*'", returnStatus: true)
//                     if (result == 0) {
//                         return false;
//                     }
//                     return true;
//                 }
//             }
            steps {
                scmSkip(deleteBuild: false, skipPattern:'.*\\[ci skip\\].*')
                sh "mvn clean install"
            }
        }
    }

    post {
        // If Maven was able to run the tests, even if some of the test.
        // failed, record the test results and archive the jar file.
        success {
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
            slackSend color: "good", message: "Message from Jenkins Pipeline"
        }
    }
}
