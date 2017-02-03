login :
==============================================================================
cf login -a api.run.pivotal.io -u mojo.jojo.mj13@gmail.com
password > S@***&&&&

cf push eureka-service-prits -b java_buildpack
OR

cf push eureka-service-prits -b https://github.com/cloudfoundry/java-buildpack.git


creting and binding a user-provided-service in cf :
===============================================================================

creating :

	cf cups eureka -p '{"url":"https://eureka-service-prits.cfapps.io/eureka/"}'
	
	cups  = create-user-provided-service
	
	OR
	cf cups SERVICE_INSTANCE -p "url"
	
	$ url > _

	enter the value of url now.

Similarly you can provide multiple key=value pairs for the service

To check the services  in the space :

cf services	

Getting services in org mojo_Org / space development as mojo.jojo.mj13@gmail.com...
OK

name     service         plan   bound apps             last operation
eureka   user-provided          config-service-prits

cf target

API endpoint:   https://api.run.pivotal.io (API version: 2.69.0)
User:           mojo.jojo.mj13@gmail.com
Org:            mojo_Org
Space:          development


cf spaces
Getting spaces in org mojo_Org as mojo.jojo.mj13@gmail.com...

name
development
service-clients-space


	
binding the serivice to your app:

.../server-config > cf bind-service config-service-prits eureka


then check the environmental variuables

   > cf env config-service-prits
   
   
   should show you the VCAP_APPLICATION and VCAP_SERVICES
   

for unbinding  > cf unbind-serivice config-service-prits eureka

to check logs :
==============================


    > cf logs <app-name> --recent
	

SERVICE BROKER STEPS :
==============================================================================================

	>	cf create-space service-clients-space -o mojo_Org

Create a service broker app as per the Implementation given in https://docs.cloudfoundry.org/services/api.html

Sample App at :  https://cf-service-hashmap.cfapps.io/cf-service-hashmap/
Services  : https://cf-service-hashmap.cfapps.io/cf-service-hashmap/v2/catalog


Backend a CF MySQL DB (Clear DB) which is bound as a Service to this App (cf-service-hashmap)
The Db service name is "hash-db", Org  : mojo_org, space : development


change the target : switch to client space that will be registering with the broker

       > cf target -s service-clients-space 

	check  > cf target 

	Sample output :
	
		API endpoint:   https://api.run.pivotal.io (API version: 2.69.0)
		User:           mojo.jojo.mj13@gmail.com
		Org:            mojo_Org
		Space:          service-clients-space

    > cf create-service-broker prits-hashmap-service-broker01 prits secret https://cf-service-hashmap.cfapps.io/cf-service-hashmap --space-scoped 
  
  	 	

	  >	cf create-service-broker prits-hashmap-service-broker01 prits secret https://cf-service-hashmap.cfapps.io/cf-service-hashmap --space-scoped

		Creating service broker prits-hashmap-service-broker01 in org mojo_Org / space service-clients-space as mojo.jojo.mj13@gmail.com...
		OK


		
	>  cf marketplace

		Sample Output:
		newrelic                         standard                                                                             Manage and monitor your apps
		prits-hashmap-service-broker01   basic                                                                                prits-hash-map - HashMap as a Service
		pubnub                           free                                                                                 Build Realtime Apps that Scale
	

CREATE SERVICE INSTANCE :
------------------------------------------------------------------------------------------------------------------------------------------	
	> 	cf create-service prits-hashmap-service-broker01 basic prits-hashmap-service01
		Creating service instance prits-hashmap-service01 in org mojo_Org / space service-clients-space as mojo.jojo.mj13@gmail.com...
		OK
	
	>	cf services
		Getting services in org mojo_Org / space service-clients-space as mojo.jojo.mj13@gmail.com...
		OK

		name                           service                          plan    bound apps   last operation
		prits-hashmap-service01   prits-hashmap-service-broker01   basic                create succeeded

CREATE SERVICE BINDINGS:
--------------------------------------------------------------------------------------------------------------------------------------------
    >	cf create-service-key prits-hashmap-service01 prits-hashmap-service-binding01
		Creating service key prits-hashmap-service-binding01 for service instance prits-hashmap-service01 as mojo.jojo.mj13@gmail.com...
		OK

    >	cf service-key prits-hashmap-service01 prits-hashmap-service-binding01
		Getting key prits-hashmap-service-binding01 for service instance prits-hashmap-service01 as mojo.jojo.mj13@gmail.com...

		{
		 "password": "secret",
		 "uri": "https://cf-service-hashmap.cfapps.io/cf-service-hashmap/prits-hashmap/sdasdsad-asdasdmasdsad-asdsad",   ????? FIX THIS 
		 "username": "prits"
		}
	


UNDOING THE CHANGES:
===========================================================================================================================


	for updating the service broker we need to first delete the service 
	   > cf delete-service-key prits-hashmap-client-service prits-hashmap-client-keys 
	   Really  ? Y
	   
	   > cf delete-service prits-hashmap-client-service
	   Really  ? Y
	   
	  then delete the service broker 
	  
	   > cf marketplace 
	If your service broker is still listed then delete it
	
	=====================================================================================================================================================
	newrelic                         standard                                                                             Manage and monitor your apps
	prits-hashmap-service-broker01   basic                                                                                prits-hash-map - HashMap as a Service
	pubnub                           free                                                                                 Build Realtime Apps that Scale
	=====================================================================================================================================================
	
	
	  > cf delete-service-broker prits-hashmap-service-broker01 (the name used while creating the service broker, we used cf create-service-broker hash-service-broker)
	  
	  

	delete a space : 
	    > cf delete-space service-clients-space
	    > cf spaces
	   
	 
	







 
  
  
  
   
   
	


 
	
	
	



