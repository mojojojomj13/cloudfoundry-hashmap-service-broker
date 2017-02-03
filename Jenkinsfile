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
      if (isUnix()) {
         mvn = ${mvnHome}/bin/mvn
      }else{
         mvn = ${mvnHome}\bin\mvn
      }
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "${mvn} -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvn}" -Dmaven.test.failure.ignore clean package/)
      }
   }
//   stage('Results') {
//       junit '**/target/surefire-reports/TEST-*.xml'
//       archive 'target/*.jar'
//   }
}
