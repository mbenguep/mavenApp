code node{
    stage('Checkout'){


        git 'https://github.com/mbenguep/mavenApp.git'
    }

    stage('Clean Package') {
        echo 'Code Quality'
        withSonarQubeEnv('s1-91') { 
          sh "/opt/maven/bin/mvn clean package"
        }
    }
    stage('SonarQube Analysis') {
        echo 'Code Quality'
        withSonarQubeEnv('s1-91') { 
          sh "/opt/maven/bin/mvn sonar:sonar"
        }
    }
        stage('Copy Artifact to Ansible-Server'){
        // getting maven home path

        sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '',
        execCommand: '''ansible-playbook /opt/docker/regapp.yml;
        sleep 10;
        ansible-playbook /opt/docker/deploy_regapp.yml''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, 
        noDefaultExcludes: false, patternSeparator: '[, ]+', 
        remoteDirectory: '/opt/docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')], 
        usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
}