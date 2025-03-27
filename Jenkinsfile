pipeline {
  agent any

  environment {
    Name = "Rohit Kumar"
  }

  parameters {
    choice choices: ['dev', 'Master'], name: 'env'
  }

  stages {


    stage("Build the Application"){
        steps {
            sh 'mvn clean package -DskipTests=true'
        }
        post {
            success {
                dir("webapp/target/")
                {
                    stash name: "maven-build", includes: "*.war"
                }
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
            stage("Testing in PROD Environment") {
                agent { label 'Master'}
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
                echo "Unstashing is started"
                unstash "maven-build"
                echo "Unstashing is completed"
            }
            sh """
            cd /var/www/html/
            jar -xvf webapp.war
            """
        }
    }

    stage("Deploy into PROD Server") {
        when { expression {params.env == 'Master'}
        beforeAgent true}
        agent {label 'Master'}

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