// Create the parameters (All, Bash, Python, C) to build choices.
// after first build refresh the jenkins page to get Build with Parameters choice
properties([parameters([choice(choices: 'All\nBash\nC\nPython', description: 'Select Languages to build', name: 'Languages')])])

pipeline {
    //agent any
     agent { node { label 'linux' } }
    stages {
    
        // Clone Sources from GitHub.
        stage('Clone Sources') {
            steps {
                checkout scm
            } 
        }
    
        
        stage('Build') {
            steps {
                echo 'Build process..'            
                sh '''
                    chmod +x *.sh
		    gcc lang_c.c -o lang_c
                    chmod +x lang_c
                '''
            }
        }

        //If you select "All", run all scripting languages and print result to results file
        stage ('All') {
            when {
                expression { params.Languages == 'All' }
            }

            steps {
                echo "Run ${params.Languages} languages"
                sh '''
		    echo "your choice: All" > results
		    python lang_py.py
		    python lang_py.py $PARAM >> results
		    bash lang_bash.sh
		    bash lang_bash.sh $PARAM >> results
                    ./lang_c
		    ./lang_c $PARAM >> results
                ''' 
            }        
        }

        //If you select "Bash", run bash scripting language and print result to results file
        stage ('Bash') {
            when {
                expression { params.Languages == 'Bash' }
            }

            steps {
                echo "Run ${params.Languages} language"
                sh '''
		    echo "your choice: Bash" > results
		    bash lang_bash.sh
		    bash lang_bash.sh $PARAM >> results
	        '''
            }        
        }
  
        //If you select "C", run c scripting language and print result to results file
        stage ('C') {
            when {
                expression { params.Languages == 'C' }
            }

            steps {
                echo "Run ${params.Languages} language"
                sh '''
		    echo "your choice: C" > results
                    ./lang_c
		    ./lang_c $PARAM >> results
                ''' 
            }        
        }
  
        //If you select "Python", run python scripting language and print result to results file
        stage ('Python') {
            when {
                expression { params.Languages == 'Python' }
            }

            steps {
                echo "Run ${params.Languages} language"
                sh '''
		    echo "your choice: Python" > results
		    python lang_py.py
		    python lang_py.py $PARAM >> results
		'''
            }        
        }

        // Save report files to your workspace folder in Results folder: include environment log, Build number and results.
        stage('Saving Results') {
            steps {
	        echo 'Saving Results process..'
                sh '''
		    report_file="${HOME}/Documents/Deployment/report"
		    mkdir -p ${HOME}/Documents/Deployment/
		    if [ -f "${report_file}" ]; then
                        echo "file ${report_file} exists"
                    else
	                touch ${report_file}
                    fi              
                    echo "Build Number: $BUILD_NUMBER" >> ${report_file}
                    cat ${WORKSPACE}/results >> ${report_file}
	            echo "#############################" >> ${report_file}
                    printenv > ${HOME}/Documents/Deployment/env_log.txt
                '''
            }
        }
    }
}
