// pipeline {
//    agent any

//    stages {
//       stage('Verify Branch') {
//          steps {
//             echo "$GIT_BRANCH"
//          }
//       }
//       stage('Docker Build') {
//          steps {
//             sh(script: 'docker compose build')
//             // powershell(script: 'docker images -a')
//             // powershell(script: """
//             //      cd azure-vote/
//             //      docker images -a
//             //      docker build -t jenkins-pipeline .
//             //      docker images -a
//             //      cd ..
//             //      """ )
//          }
//       }
//       // stage('Start App') {
//       //    steps {
//       //       sh(script: 'docker compose up -d')
//       //    }
//       // }
//    //    stage('Run Tests') {
//    //       steps {
//    //          echo "Running tests)"
//    //       }
//    //       post {
//    //          success {
//    //             echo "Tests passed! :)"
//    //          }
//    //          failure {
//    //             echo "Tests failed :("
//    //          }
//    //       }
//    //    }
//    // }
//    // post {
//    //    always {
//    //       sh(script: 'docker compose down')
//    //    }
//    // }
// }
// }

pipeline {
   agent any
 
   environment{
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
  }
 
   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker compose build')
            // powershell(script: 'docker images -a')
            // powershell(script: """
            //      cd azure-vote/
            //      docker images -a
            //      docker build -t jenkins-pipeline .
            //      docker images -a
            //      cd ..
            //      """ )
         }
      }
      stage('Start App') {
         steps {
            sh(script: 'docker compose up -d')
         }
      }
      stage('Run Tests') {
         steps {
            echo "Running tests)"
         }
         post {
            success {
               echo "Tests passed! :)"
            }
            failure {
               echo "Tests failed :("
            }
         }
      }
      stage ('Login to DockerHub') {
 
                       steps {
                           sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                       }
 
                  }
               
      stage('Docker Push') {
         steps {
            echo "Running in $WORKSPACE"
            dir("$WORKSPACE/azure-vote"){
            script{
           
                     def image = docker.build('sauratan/jenkins-course')
                     image.push()
              }
          }
         }
      }
     
   }
   post {
      always {
         sh(script: 'docker compose down')
      }
   }
}