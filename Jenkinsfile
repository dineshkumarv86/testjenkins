pipeline {
  agent {
    docker {
      image 'gmacario/build-yocto'
    }

  }
  

  stages {
 
    stage('Collect resources'){
            steps{
                
                  sh "  git clone https://github.com/adlink/meta-adlink-sema"
               sh "git clone https://github.com/adlink/meta-adlink-x86-64bit -b sumo"
                checkout(
                   [$class: 'GitSCM', 
                   branches: [[name: '$branch'],
                   [name: 'sumo']], 
                   doGenerateSubmoduleConfigurations: false, 
                   extensions: [
                       [$class: 'RelativeTargetDirectory', 
                       relativeTargetDir: 'poky']], 
                      gitTool: 'Default', 
                   submoduleCfg: [], 
                   userRemoteConfigs: [[url: 'git://git.yoctoproject.org/poky.git ']]]
                )
                
                sh "git clone git://git.openembedded.org/meta-openembedded -b $branch"
                checkout(
                   [$class: 'GitSCM', 
                   branches: [[name: '$branch'],
                   [name: 'sumo']], 
                   doGenerateSubmoduleConfigurations: false, 
                   extensions: [
                       [$class: 'RelativeTargetDirectory', 
                       relativeTargetDir: 'meta-openembedded']], 
                                        gitTool: 'Default',   
                   submoduleCfg: [], 
                   userRemoteConfigs: [[url: 'git://git.openembedded.org/meta-openembedded.git']]]
                )
                
                sh "git clone git://git.yoctoproject.org/meta-intel -b $branch"
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '$branch'],
                    [name: 'sumo']], 
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'RelativeTargetDirectory', 
                    relativeTargetDir: 'meta-intel']], 
                   gitTool: 'Default',   
                    submoduleCfg: [], 
                    userRemoteConfigs: [[url: 'git://git.yoctoproject.org/meta-intel']]]
                )
                
                sh "git clone https://github.com/meta-qt5/meta-qt5.git -b $branch"
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '$branch'],
                    [name: 'sumo']], 
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'RelativeTargetDirectory', 
                    relativeTargetDir: 'meta-qt5']], 
                   gitTool: 'Default',   
                    submoduleCfg: [], 
                    userRemoteConfigs: [[url: ' https://github.com/meta-qt5/meta-qt5.git']]]
                )
                
                sh "git clone https://github.com/jiazhang0/meta-secure-core.git"
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: 'sumo']], 
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'RelativeTargetDirectory', 
                    relativeTargetDir: 'meta-secure-core']], 
                   gitTool: 'Default',   
                    submoduleCfg: [], 
                    userRemoteConfigs: [[url: 'https://github.com/jiazhang0/meta-secure-core.git']]]
                )
                
                sh "git clone git://git.yoctoproject.org/meta-virtualization -b $branch"
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '$branch'],
                    [name: 'sumo']], 
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'RelativeTargetDirectory', 
                    relativeTargetDir: 'meta-virtualization']], 
                   gitTool: 'Default',   
                    submoduleCfg: [], 
                    userRemoteConfigs: [[url: 'git://git.yoctoproject.org/meta-virtualization']]]
                )

                sh "ls"
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
