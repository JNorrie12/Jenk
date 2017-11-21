pipeline {
  agent any
  stages {
      stage('Clone Branch'){
         steps {
            echo "We are currently working on branch: ${env.BRANCH_NAME}"
            sh """
		cd /home/jnorrie
                if [ -d "${env.BRANCH_NAME}/Jenk" ]; then
                rm -rf ${env.BRANCH_NAME}/Jenk
                echo "build already exists, cleaning..."
                fi
		mkdir ${env.BRANCH_NAME}
		cd ${env.BRANCH_NAME}
	   	git clone -b ${env.BRANCH_NAME} https://github.com/JenkTest/Jenk
		"""
	
		
		
         }
    }
    stage('Build Cmake Clean'){
        steps{
            
            sh  """
                cd /home/jnorrie/Jenk
                if [ -d "build" ]; then
                rm -rf build
                echo "build already exists, cleaning..."
                fi
                mkdir build
                echo "Building cmake project"
                cd build && cmake ..
                echo "Build complete, cleaning project"
                cd ..
                rm -rf build
                """
        }
    }
    stage('Build Cmake'){
        steps{
            
            sh  """
                cd /home/jnorrie
		cmake ${env.BRANCH_NAME}/Jenk
                echo "Build complete, cleaning project"
		echo "Build Removed"
                """
        }
    }
      stage('Test') {
        steps {
          script {
              // change to 'UNSTABLE' OR 'FAILED' to test the behaviour 
              currentBuild.result = 'SUCCESS'
          }
        }
      }
  }
  post {
        always {
          step([$class: 'Mailer',
            notifyEveryUnstableBuild: true,
            recipients: 'jenkenstest@gmail.com',
            sendToIndividuals: true])
        }
  }
}

