node {
   def mvnHome
   stage('Clone') { // for display purposes
      // Get some code from a GitHub repository
      checkout scm
   }
   
   stage('Build') {
      // Run the maven build
      withMaven(
        // Maven installation declared in the Jenkins "Global Tool Configuration"
        maven: 'M3',
        // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
        // We recommend to define Maven settings.xml globally at the folder level using
        // navigating to the folder configuration in the section "Pipeline Maven Configuration / Override global Maven configuration"
        // or globally to the entire master navigating to  "Manage Jenkins / Global Tools Configuration"
        mavenSettingsConfig: 'sword-settings',
        mavenLocalRepo: '.repo') {
            sh 'mvn clean package'
      }
   }

   stage('Deploy Staging') {
    steps {
      // We deploy to sword repositories for further integration tests
      sh """
        mvn -B deploy -DskipTests -DretryFailedDeploymentCount=5 -DaltDeploymentRepository=myrepo://my.staging.maven.repo -Ddocker.push.registry=https://my.staging.docker.registry
      """
    }
  }

}