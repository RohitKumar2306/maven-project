pipeline {
  agent any
  environment {
    Name = "Rohit Kumar"
  }
  parameters {
    string defaultValue: 'Rohit Kumar', description: 'First Name', name: 'FirstName'
    choice choices: ['1', '2', '3', '4', '5'], description: 'Example learning', name: 'Choices'
  }   

  stages {
    stage("Validate Application") {
      steps {
        echo "This is build stage"
        echo "My Name is $Name ${params.LastName} ${params.FirstName}"
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