pipeline{
    agent any
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean verify -Dtest=OQONightlyTest -DskipTests=true -Dmaven.javadoc.skip=true -B -V'
            }
			}
 	  stage('Docker Up'){
            steps{
				sh 'docker compose up -d --scale chrome=4'
            }
			}

	  stage('Run Tests'){
            steps{
				sh 'mvn verify -Dtest=OQONightlyTest -Dthreads=4'
            }
			}

     stage('Reports') {
         steps {
         script {
                 allure([
                         includeProperties: false,
                         jdk: '',
                         properties: [],
                         reportBuildPolicy: 'ALWAYS',
                         results: [[path: 'target/allure-results']]
                 ])
         }
         }
     }

    } // end of stages

    post {
        always{
        sh 'docker compose down'
        }//end of always
    } // end of post
}//end of pipeline


