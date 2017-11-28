def buildBool = 0
def cloneBool = 0
def time1 = 0
def time2 = 0
def time3 = 0

def clone(BRANCH) {
	   sh """
		cd /home/jnorrie
                if [ -d "${BRANCH}" ]; then
                rm -rf ${BRANCH}
                echo "build already exists, cleaning..."
                fi
		mkdir ${BRANCH}
		cd ${BRANCH}
	   	git clone -b ${BRANCH} https://github.com/JenkTest/Jenk
		echo "worked"
		"""
	}

def build(BRANCH) {
		sh  """
        cd /home/jnorrie/${BRANCH}
	cmake Jenk
        echo "Build complete, cleaning project"
	cd ..
	rm -rf ${BRANCH}
	echo "Build Removed"
        """
	}

def pythonReport(CLONE, BUILD) {
	sh """
		cd /home/jnorrie/archive
		python -c 'from reportMaker import mainCollect; print mainCollect(${env.BUILD_NUMBER},${CLONE},${BUILD}, "${env.GIT_BRANCH}" +"_report.yaml")' 
		"""

	}


pipeline {
agent{
	label 'NumeroUno'
	}
	
  stages {
      stage('Clone Branch'){
	      steps {
		     time1 = ${env.BUILD_TIMESTAMP}
		     echo "We are currently working on branch: ${env.BRANCH_NAME}" 
		     clone(env.BRANCH_NAME)
		     time2 = ${env.BUILD_TIMESTAMP}
         	}
  		post {
	  		success {
				echo "Branch cloned."
				      script {cloneBool = 1}
				}
			failure {echo "Failure whilst cloning branch."}
  		}	  
  	}  
	  
    stage('Build Cmake'){
    	steps{
		echo "Building branch."
		build(env.BRANCH_NAME)
		time3 = ${env.BUILD_TIMESTAMP}
	}
	    post{
		   
		    success{
			    echo "Branch built."
				script{buildBool = 1}
				 
		    	}
		    failure {echo "Error in build."}
	    }
  	}
  
  }

	post{
		always{
			pythonReport(cloneBool, buildBool)
		}
	}
	
	
}

