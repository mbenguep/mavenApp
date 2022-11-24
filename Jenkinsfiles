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
    stage('Copy Artifact to Ansible-Server'){
        // getting maven home path

        sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
        execCommand: '''cd /opt/docker;
        ansible-playbook create_image.yml''', execTimeout: 900000000, flatten: false, makeEmptyDirs: false, 
        noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, 
        removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')], usePromotionTimestamp: false, 
        useWorkspaceInPromotion: false, verbose: false)])

}
    stage('Deploy the onto openshift'){
        // getting maven home path

    sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
    execCommand: 'ansible-playbook -i /opt/docker/inventory /opt/docker/kube_deploy.yml', execTimeout: 3000000, flatten: false, 
    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, 
    removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

}

    stage('Deploy the onto openshift'){
        // getting maven home path

    sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
    execCommand: 'ansible-playbook -i /opt/docker/inventory /opt/docker/kube_service.yml', execTimeout: 3000000, flatten: false, 
    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, 
    removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

}

    stage('create service onto openshift'){
        // getting maven home path

    sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
    execCommand: 'ansible-playbook -i /opt/docker/inventory /opt/docker/kube_route.yml', execTimeout: 3000000, flatten: false, 
    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, 
    removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

}

    stage('creating route to access the App'){
        // getting maven home path

    sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
    execCommand: 'ansible-playbook -i /opt/docker/inventory /opt/docker/kube_route.yml', execTimeout: 3000000, flatten: false, 
    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, 
    removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

}

    stage('Restart the Deployment'){
        // getting maven home path

    sshPublisher(publishers: [sshPublisherDesc(configName: 'dockerhost', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
    execCommand: 'oc rollout restart deployment mamadou-memapp -n deploy-mem', execTimeout: 3000000, flatten: false, 
    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, 
    removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

}
}