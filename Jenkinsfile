pipeline {
  agent any
  environment {
    Name = "Rohit Kumar"
  }
  parameters {
    string defaultValue: 'Chintamani', description: 'LastName = Chintamani', name: 'LastName'
  }

  stages {
    stage("Validate Application") {
      steps {
        echo "This is build stage"
        echo "My Name is $Name ${params.LastName = 'CHINTHAMANI'}"
        sh "mvn validate"
      }
    }

    // stage("Package the Application") {
    //   steps {
    //     echo "This is packaging stage"
    //     sh "mvn clean package"
    //   }
    //   post {
    //     success {
    //         echo "BUILD IS SUCCESSFULL"
    //         archiveArtifacts artifacts: '**/target/*.war'
    //     }
    //   }
    // }
  }
}