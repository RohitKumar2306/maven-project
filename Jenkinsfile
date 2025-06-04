pipeline {
  agent any

  environment {
    Name = "Rohit Kumar"
  }

  stages {


    stage("Build the Application"){
        steps {
            sh 'mvn clean package -DskipTests=true'
            dir("webapp/target/"){
                sh "pwd"
                stash includes: "*.war", name: 'maven-build'
            }
        }
        post {
            success {
                echo "BUILD IS SUCCESSFULL"
            }
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
        }
    }

    stage("Deploy into DEV Server") {
        when { expression {params.env == 'dev'}
        beforeAgent true}
        agent {label 'dev'}

        steps {
            dir("/var/www/html") {
                script {
                    try {
                        unstash 'maven-build'
                    } catch (Exception e) {
                        echo "Failed to unstash: ${e.getMessage()}"
                        throw e
                    }
                }
            }
            sh """
            cd /var/www/html/
            jar -xvf webapp.war
            """
        }
    }
  }
}