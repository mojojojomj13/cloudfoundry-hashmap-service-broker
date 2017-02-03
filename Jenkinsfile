node {
   def mvnHome
   def mvn
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/mojojojomj13/cloudfoundry-hashmap-service-broker.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M3'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "${mvnHome}/bin/mvn clean install -Dmaven.test.skip=true"
      } else {
         bat(/"${mvnHome}\bin\mvn clean install -Dmaven.test.skip=true"/)
      }
   }
//   stage('Results') {
//       junit '**/target/surefire-reports/TEST-*.xml'
//       archive 'target/*.jar'
//   }
}
