pipeline {
    agent any
    stages {
      stage('Deploy') {
        steps {
          // One or more steps need to be included within the steps block.
		  echo '\'Running build automation\''
		  sh './gradlew build --no-daemon'
		  archiveArtifacts artifacts: 'dist/trainSchedule.zip', followSymlinks: false
        }
      }
    
      stage('DeployToStaging') {
        steps {
          // One or more steps need to be included within the steps block.
		  withCredentials([usernamePassword(credentialsId: 'webserver-login', passwordVariable: 'USERPASS', usernameVariable: 'USERNAME')]) {
           // some block
		   sshPublisher failOnError: true, publishers: [sshPublisherDesc(configName: 'stage-server', sshCredentials: [encryptedPassphrase: '$USERPASS', key: '', keyPath: '', username: '$USERNAME'], transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sudo unzip /tmp/trainSchedule.zip -d /opt/train-schedule', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/tmp', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/trainSchedule.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)]
          }
        }
    
        when {
          branch 'master'
        }
      }
    
      stage('DeployToProduction') {
        steps {
          // One or more steps need to be included within the steps block.
		  withCredentials([usernamePassword(credentialsId: 'webserver-login', passwordVariable: 'USERPASS', usernameVariable: 'USERNAME')]) {
		    sshPublisher failOnError: true, publishers: [sshPublisherDesc(configName: 'prod-server', sshCredentials: [encryptedPassphrase: '$USERPASS', key: '', keyPath: '', username: '$USERNAME'], transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sudo unzip /tmp/trainSchedule.zip -d /opt/train-schedule', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/tmp', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/trainSchedule.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)]
		  }
        }
    
        when {
          branch 'master'
        }
      }

   }

}
