pipeline {
  agent any
   stages {
    stage ('Build') {
      steps {
        sh '''#!/bin/bash
        python3 -m venv test3
        source test3/bin/activate
        pip install pip --upgrade
        pip install -r requirements.txt
        export FLASK_APP=application
        flask run &
        '''
     }
      
     post{
        success { 
          slackSend channel: 'jenkinsnotifications',
                    color: 'good',
                    message: "The build completed successfully."
        }
        failure  {
          slackSend channel: 'jenkinsnotifications',
                    color: 'warning',
                    message: "The build FAILED"
        }   
     }   
   }
    stage ('test') {
      steps {
        sh '''#!/bin/bash
        source test3/bin/activate
        py.test --verbose --junit-xml test-reports/results.xml
        ''' 
      }
    
      post{
        always {
          junit 'test-reports/results.xml'
        }
        success { 
          slackSend channel: 'jenkinsnotifications',
                    color: 'good',
                    message: "The test completed successfully."
        }
        failure  {
          slackSend channel: 'jenkinsnotifications',
                    color: 'warning',
                    message: "The test FAILED"
        }   
      }  
    }
     stage ('Deploy') {
       steps {
         sh '/var/lib/jenkins/.local/bin/eb deploy url-shortener-main-dev'
       }
       post{
        success { 
          slackSend channel: 'jenkinsnotifications',
                    color: 'good',
                    message: "The application has been successfully deployed."
        }
        failure  {
          slackSend channel: 'jenkinsnotifications',
                    color: 'warning',
                    message: "The application did not deploy successfully"
        }
      }    
     }   
   }
 }
