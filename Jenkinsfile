node{
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

    stage('Create docker image onto dockerhub') {

        sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', 
        transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook create_image.yml',
        execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+',
        remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')],
        usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    }

        stage('Copy Artifact to Ansible-Server and deploy'){

        sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '',
        execCommand: '''ansible-playbook -i /opt/docker/inventory /opt/docker/kube_deploy.yml;
        sleep 10;
        ansible-playbook -i inventory kube_service.yml''', execTimeout: 1200000, flatten: false, makeEmptyDirs: false, 
        noDefaultExcludes: false, patternSeparator: '[, ]+', 
        remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')], 
        usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
}

