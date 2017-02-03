node ('main') {
   def mvnHome
   stage('Preparation') {
      git 'https://github.com/mojojojomj13/cloudfoundry-hashmap-service-broker.git'         
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
}
