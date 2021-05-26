pipeline {
    agent any
    parameters {
    choice (choices: ['dev', 'qa'], description: '', name: 'ENVIRONMENT')

    string (defaultValue: 'master', description: '', name: 'BranchName', trim: false)

    string (defaultValue: '', description: '', name: 'GitCredentials', trim: false)

    string (defaultValue: 'https://github.com/mdkadar/java_calc.git', description: '', name: 'GitURL', trim: false)

    string (defaultValue: '', description: 'sshPublisher Configuration name', name: 'configName', trim: false)

    string (defaultValue: '', description: '', name: 'removePrefix', trim: false)

    string (defaultValue: '', description: '', name: 'sourceFiles', trim: false)

    string (defaultValue: '', description: '', name: 'remoteDirectory', trim: false)

    }

    stages {
       
        stage ('Build') {
            
            steps {
                 withMaven(maven: 'mvw') {
                 git branch: '$BranchName', credentialsId: '$GitCredentials', url: '$GitURL'
                 sh 'mvn package' 
            }
        }
    }
    stage('Deploy to Qa') {
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
            
            stage('Deploy to Dev') {
            when {
                expression { 
                   return params.ENVIRONMENT == 'dev'
                }
            }
            steps {
                    sh """
                    echo "deploy to dev"
                    """
                }
            }
   }
}
