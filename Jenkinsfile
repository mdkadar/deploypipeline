pipeline {
    agent any
    parameters {
    choice (choices: ['ubuntu', 'qa'], description: '', name: 'ENVIRONMENT')

    string (defaultValue: 'master', description: '', name: 'BranchName', trim: false)

    string (defaultValue: '', description: '', name: 'GitCredentials', trim: false)

    string (defaultValue: '', description: '', name: 'GitURL', trim: false)

    string (defaultValue: '', description: 'sshPublisher Configuration name', name: 'configName', trim: false)

    string (defaultValue: '', description: '', name: 'removePrefix', trim: false)

    string (defaultValue: '', description: '', name: 'sourceFiles', trim: false)

    string (defaultValue: '', description: '', name: 'remoteDirectory', trim: false)

    }

    stages {
       
        stage ('Build') {
            
            steps {
                 //withMaven(maven: 'mvn') {
                 git branch: '$BranchName', credentialsId: '$GitCredentials', url: '$GitURL'
                 bat "mvn clean install"
                 bat "mvn clean package"
            //}
        }
    }
               
            stage('Deploy to ubuntu') {
            when {
                expression { 
                   return params.ENVIRONMENT == 'ubuntu'
                }
            }
            steps {
                    sh """
                    echo "deploy to dev"
                    """
                }
            }
        stage('Deploy to qa') {
            when {
                expression { 
                   return params.ENVIRONMENT == 'qa'
                }
            }
            steps {
                    
                    sshPublisher(publishers: [sshPublisherDesc(configName: '$configName', 
                    transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', 
                    execTimeout: 120000, flatten: false, 
                    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', 
                    remoteDirectory: '$remoteDirectory', remoteDirectorySDF: false, 
                    removePrefix: '$removePrefix', 
                    sourceFiles: '$sourceFiles')], 
                    usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                }
            }
   }
}
