node("maven") {
  stage("Build Fat JAR") {
    git url: "https://github.com/burrsutter/catalog-spring-boot"
    sh "mvn clean package"
    stash name:"jar", includes:"target/catalog-1.0-SNAPSHOT.jar"
  }

  stage("Wassup") {
      echo "Wassup"
  }
  stage("Build Image") {
    unstash name:"jar"
    sh "oc start-build catalog-s2i --from-file=target/catalog-1.0-SNAPSHOT.jar"
    openshiftVerifyBuild bldCfg: "catalog-s2i", waitTime: '19', waitUnit: 'min'
  }

  stage("Deploy") {
    openshiftDeploy deploymentConfig: "catalog"
  }
}