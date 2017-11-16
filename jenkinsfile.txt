pipeline {
  agent any
  stages {
      stage('Build Python'){
         steps {
            echo 'hello' 
            sh 'python /home/jnorrie/Jenk/HelloWorld.py'
         }
    }
    stage('Build Cmake'){
        steps{
            
            sh  '''
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
                '''
        }
    }
    stage('Build Cmake2'){
        steps{
            
            sh  '''
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
                '''
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
