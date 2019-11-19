pipeline {
  agent {
    docker {
      image 'gmacario/build-yocto'
    }

  }
  

  stages {
  stage('clone') { // for display purposes
    steps{
    // Get some code from a GitHub repository
    checkout([$class: 'GitSCM',
        branches: [[name: '*/sumo']],
        extensions: [[$class: 'CloneOption', timeout: 120]],
        gitTool: 'Default', 
        userRemoteConfigs: [[url: 'git://git.yoctoproject.org/poky.git']]
             ]) }
}    
    stage('collect resource') {
      steps {
       
        checkout([$class: 'GitSCM', 
                          branches: [[name: '$branch'],
                   [name: '5ddf7fff992b065ee512878d2fe65f3e35d818cf']], 
                   doGenerateSubmoduleConfigurations: false, 
                   extensions: [
                       [$class: 'RelativeTargetDirectory', 
                       relativeTargetDir: 'poky']], 
                   submoduleCfg: [], 
                   userRemoteConfigs: [[url: 'git://git.yoctoproject.org/poky.git']]]
          sh "git clone git://git.openembedded.org/meta-openembedded -b $branch"
          checkout([$class: 'GitSCM', 
                             branches: [[name: '$branch'],
                             [name: '8760facba1bceb299b3613b8955621ddaa3d4c3f']], 
                             doGenerateSubmoduleConfigurations: false, 
                             extensions: [
                                   [$class: 'RelativeTargetDirectory', 
                                   relativeTargetDir: 'meta-openembedded']], 
                               submoduleCfg: [], 
                               userRemoteConfigs: [[url: 'git://git.openembedded.org/meta-openembedded']]])
            sh "git clone git://git.yoctoproject.org/meta-intel -b $branch"
            checkout([
                                  $class: 'GitSCM', 
                                  branches: [[name: '$branch'],
                                  [name: '90af97d23fb2a56187c2fe2a3f4f4190d7cc2605']], 
                                  doGenerateSubmoduleConfigurations: false,
                                  extensions: [[$class: 'RelativeTargetDirectory', 
                                  relativeTargetDir: 'meta-intel']], 
                                  submoduleCfg: [], 
                                  userRemoteConfigs: [[url: 'git://git.yoctoproject.org/meta-intel']]])
              sh "git clone https://github.com/meta-qt5/meta-qt5.git -b $branch"
              checkout([
                                    $class: 'GitSCM', 
                                    branches: [[name: '$branch'],
                                    [name: 'd4e7f73d04e8448d326b6f89908701e304e37d65']], 
                                    doGenerateSubmoduleConfigurations: false,
                                    extensions: [[$class: 'RelativeTargetDirectory', 
                                    relativeTargetDir: 'meta-qt5']], 
                                    submoduleCfg: [], 
                                    userRemoteConfigs: [[url: ' https://github.com/meta-qt5/meta-qt5.git']]])
                sh 'git clone https://github.com/jiazhang0/meta-secure-core.git'
                checkout([
                                      $class: 'GitSCM', 
                                      branches: [[name: '1b35fd45a58ef015b52a3df4b39048f2ac1ffbe3']], 
                                      doGenerateSubmoduleConfigurations: false,
                                      extensions: [[$class: 'RelativeTargetDirectory', 
                                      relativeTargetDir: 'meta-secure-core']], 
                                      submoduleCfg: [], 
                                      userRemoteConfigs: [[url: 'https://github.com/jiazhang0/meta-secure-core.git']]])
                  sh "git clone git://git.yoctoproject.org/meta-virtualization -b $branch"
                  checkout([
                                        $class: 'GitSCM', 
                                        branches: [[name: '$branch'],
                                        [name: 'ed2038c935777d1336c17989d454f4e9c95fea7f']], 
                                        doGenerateSubmoduleConfigurations: false,
                                        extensions: [[$class: 'RelativeTargetDirectory', 
                                        relativeTargetDir: 'meta-virtualization']], 
                                        submoduleCfg: [], 
                                        userRemoteConfigs: [[url: 'git://git.yoctoproject.org/meta-virtualization']]])
                    sh "git clone https://github.com/adlink/meta-adlink-x86-64bit -b $branch"
                    sh 'git clone https://github.com/adlink/meta-adlink-sema'
                    sh 'ls'
                  }
                }

                stage('Set Config') {
                  steps {
                    sh 'sed -i \'s/"4"/"14"/g\' meta-adlink-x86-64bit/conf/Adlink-conf/local.conf'
                    sh 'sed -i \'s/"-j 4"/"-j 14"/g\' meta-adlink-x86-64bit/conf/Adlink-conf/local.conf'
                    sh 'cp $PWD/meta-adlink-x86-64bit/conf/Adlink-conf/bblayers.conf $PWD/meta-adlink-x86-64bit/conf/Adlink-conf/bblayers.conf.sample'
                    sh 'cp $PWD/meta-adlink-x86-64bit/conf/Adlink-conf/local.conf $PWD/meta-adlink-x86-64bit/conf/Adlink-conf/local.conf.sample'
                  }
                }

                stage('Cook') {
                  steps {
                    sh "export TEMPLATECONF=$PWD/workspace/LeonYoctoTest2/meta-adlink-x86-64bit/conf/Adlink-conf/&&source poky/oe-init-build-env&&bitbake core-image-minimal-initramfs"
                  }
                }

              }
              environment {
                branch = 'sumo'
              }
              post {
                always {
                  archiveArtifacts(artifacts: 'build/tmp/deploy/images/intel-corei7-64/*.*.*,build/tmp/deploy/images/intel-corei7-64/*.*', onlyIfSuccessful: true)
                  cleanWs()
                }

              }
              options {
                skipStagesAfterUnstable()
              }
              triggers {
                pollSCM('H H * * *')
              }
            }
