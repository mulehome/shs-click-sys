node {
   def projectName="shs-click-sys"
   def mvnHome
   stage('Preparation') { // for display purposes
   sh 'echo $BRANCH_NAME'
      git url: "https://github.com/SearsHomeServices/${projectName}.git"
      sh "git clean -f"
      sh "git reset --hard"
      sh "git checkout origin/$BRANCH_NAME"
   } 
    
   stage('Build') {
      if ("$BRANCH_NAME"=='development') {
            withMaven(maven : 'hs_mvn_3.3.9', mavenSettingsConfig: 'c28f6d33-8917-4d40-8824-5b882855c1ef',
        mavenLocalRepo: '.repository') {
                    sh 'mvn clean deploy -DskipMunitTests'
                }
      } else if ("$BRANCH_NAME"=='master') {
         def pom = readMavenPom file: 'pom.xml'
         POM_VERSION = readMavenPom().getVersion()
        BUILD_RELEASE_VERSION = readMavenPom().getVersion().replace("-SNAPSHOT", "")
        echo POM_VERSION
        echo BUILD_RELEASE_VERSION
         def versionList = BUILD_RELEASE_VERSION.tokenize(".")
         def newRelease = Eval.me(versionList[2])+1
         echo ''+newRelease
         def version = "${BUILD_RELEASE_VERSION}"
         stage('masterRelease') {
            withMaven(maven : 'hs_mvn_3.3.9', mavenSettingsConfig: 'c28f6d33-8917-4d40-8824-5b882855c1ef', mavenLocalRepo: '.repository') {
                sh "mvn clean versions:set -DnewVersion=${BUILD_RELEASE_VERSION}"
                sh 'mvn clean deploy -DskipMunitTests'
            }
         }
         stage('tag') {
            sh "git tag -f ${projectName}-${version} -m 'JenkinsRelease'"
            sh "git push -u origin master --tag"
         }
         stage('updatedevelopmentPOMVersion') {
            sh "git checkout ."
            sh "git checkout development"
            sh "git clean -f"
            sh "git reset --hard"
            sh "git checkout ."
            sh "git pull origin development"
            withMaven(maven : 'hs_mvn_3.3.9', mavenSettingsConfig: 'c28f6d33-8917-4d40-8824-5b882855c1ef', mavenLocalRepo: '.repository') {
                sh 'mvn clean versions:set -DnewVersion=${versionList[0]}.${versionList[1]}.${newRelease}-SNAPSHOT'
            }
            sh "git add -- pom.xml"
            sh "git commit -m 'jenskinsChangingVersion'"
            sh "git push -u origin development"
         }
      }
   }
}
