
pipeline 
{
	
	parameters 
	{
  		string defaultValue: 'dev', description: 'Based on environment config will be taken', name: 'environment'
		string defaultValue: 'abcd', description: 'secure encrypted key used to sensitive data encoding', name: 'encryptedKey'
		string defaultValue: '-dev', description: 'use in application naming convention', name: 'suffixEnv'
		string defaultValue: 'DEV', description: 'Runtime manager environment', name: 'cloudhubEnv'
	}
	agent any
	

	stages 
	{
		stage('Build Application') 
		{
		
			steps 
			{
				echo "Environment is"
				echo "${environment}"
				bat 'mvn clean install -DskipTests'
			
			}
		
		}
		
		stage('Test') 
		{
			environment {
        			ENV_NAME = "${environment}"
					SECURE_KEY = "${encryptedKey}"
    			}
			steps 
			{
				
			
				echo 'Application in Testing Phase… '
				echo "${environment}"
				bat "mvn test -Dmule.env=${environment} -Dmule.encryptionKey=${encryptedKey} -Dapp.coverage=60"
			
			}
		
		}
		
		stage('Deploy CloudHub') 
		{
		
			environment 
			{ 
				ANYPOINT_CREDENTIALS = credentials('anypointPlatforms')
		
			}
		
			steps 
			{
			
				echo 'Deploying mule project due to the latest code commit…'
				
				echo 'Deploying to the configured environment….'
				
				bat "mvn package deploy -DmuleDeploy -DskipTests -Dmule.env=${environment} -Dmule.encryptionKey=${encryptedKey} -Dapp.coverage=60 -Denv.name=${cloudhubEnv} -Danypoint.uri=https://anypoint.mulesoft.com -Dmule.version=4.4.0 -Dcloudhub.user=${ANYPOINT_CREDENTIALS_USR} -Dcloudhub.password=${ANYPOINT_CREDENTIALS_PSW} -Dcloudhub.workerType=MICRO -Dcloudhub.workerCount=1 -Dcloudhub.region=us-east-2 -Danypoint.monitoring=false -Denv.suffix=${suffixEnv}"
				
			}
		
		}
	
	}

}