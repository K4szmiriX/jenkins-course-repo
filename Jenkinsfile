// pipeline {
//     agent any
// //    triggers { cron('* * * * *') }
//
//     tools {
//         // Install the Maven version configured as "M3" and add it to the path.
//         maven "maven-3"
//         jdk "jdk-17"
//     }
//
//
//     stages {
//         stage('Checkout') {
//             steps {
//                 git url: 'https://github.com/K4szmiriX/jenkins-course-repo.git', branch: 'master'
//             }
//         }
//
//         stage('Build') {
// //             when {
// //                 expression {
// //                     result = sh (script: "git log -1 | grep '.*\\[ci skip\\].*'", returnStatus: true)
// //                     if (result == 0) {
// //                         return false;
// //                     }
// //                     return true;
// //                 }
// //             }
//             steps {
//                 scmSkip(deleteBuild: false, skipPattern:'.*\\[ci skip\\].*')
//                 sh "mvn clean install"
//             }
//         }
//     }
//
//     post {
//         // If Maven was able to run the tests, even if some of the test.
//         // failed, record the test results and archive the jar file.
//         success {
//             junit '**/target/surefire-reports/TEST-*.xml'
//             archiveArtifacts 'target/*.jar'
//             slackSend color: "good", message: "Message from Jenkins Pipeline"
//         }
//     }
// }


properties([
  parameters([
    [
      $class: 'ChoiceParameter',
      choiceType: 'PT_SINGLE_SELECT',
      name: 'Environment',
      script: [
        $class: 'ScriptlerScript',
        scriptlerScriptId:'Environments.groovy'
      ]
    ],
    [
      $class: 'CascadeChoiceParameter',
      choiceType: 'PT_SINGLE_SELECT',
      name: 'Host',
      referencedParameters: 'Environment',
      script: [
        $class: 'ScriptlerScript',
        scriptlerScriptId:'HostsInEnv.groovy',
        parameters: [
          [name:'Environment', value: '$Environment']
        ]
      ]
   ]
 ])
])

pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo "${params.Environment}"
        echo "${params.Host}"
      }
    }
  }
}