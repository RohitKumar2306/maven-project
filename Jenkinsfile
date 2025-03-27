pipeline {
  agent any
  
  environment {
    Name = "Rohit Kumar"
  }

  parameters {
    choice choices: ['dev', 'Master'], name: 'env'
  }

  stages {
    // stage("Validate Application") {
    //     parallel {    
    //         stage ("Parallelly Validating the Build") {
    //             steps {
    //                 echo "This is build stage"
    //                 echo "My Name is $Name ${params.LastName} ${params.FirstName}"
    //                 sh "mvn validate"
    //             }
    //         }
    //         stage("Parallelly Package the Application") {
    //             steps {
    //                 echo "This is packaging stage"
    //                 sh "mvn clean package"
    //             }
    //             post {
    //                 success {
    //                     echo "BUILD IS SUCCESSFULL"
    //                     archiveArtifacts artifacts: '**/target/*.war'
    //                 }
    //             }
    //         }
    //     }
    // }


    stage("Build the Application"){
        steps {
            sh 'mvn clean package -DskipTests = true'
        }
    }

    stage("Testing the Application") {
        parallel {
            stage("Testing in DEV Environment") {
                agent { label 'dev'}
                steps {
                    sh 'mvn test'
                }
            }
            stage("Testing in PROD Environment") {
                agent { label 'Master'}
                steps {
                    sh 'mvn test'
                }
            }
        }
        post {
            success {
                dir("webapp/target/")
                {
                    stash name: "maven-build", includes: "*.war"
                }
                echo "BUILD IS SUCCESSFULL"
                archiveArtifacts artifacts: '**/target/*.war'
            }
        }
    }

    stage("Deploy into DEV Server") {
        when { expression {params.env == 'dev'}
        beforeAgent true}
        agent {label 'dev'}

        steps {
            dir("/var/www/html") {
                unstash "maven-build"
            }
            sh """
            cd /var/www/html/
            jar -xvf webapp.war
            """
        }
    }

    stage("Deploy into DEV Server") {
        when { expression {params.env == 'dev'}
        beforeAgent true}
        agent {label 'dev'}

        steps {
            dir("/var/www/html") {
                unstash "maven-build"
            }
            sh """
            cd /var/www/html/
            jar -xvf webapp.war
            """
        }
    }
  }
}