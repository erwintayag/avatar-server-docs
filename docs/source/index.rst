Welcome to avatar-server-pack documentation!
===================================

.. note::

   This project is under active development.

Created - 20220126
Last Modified - 20220304

.. toctree::

+ copy prod-sample folder
+ overwrite DBs with blank copies (from prod\blank)
+ update .env 
	- correct IP address
	- available Port number
+ update d23-variables.env
	- client_id
	- client_secret
	- base_path
+ update variables.env
	- DB_NAME
	- API_NAME
	- API_SECRET
	- NEURONS_URL
	- TERMINALS_URL
	- ANONYMOUS_USER_ID
		- used as guid of an anonymous user
		- set to empty string to register owner creator in variables.env
			- this will instruct identity-access to use neuronid as authorid (owner neuron)
+ update d23.db
	! Make sure to specify proper Url values otherwise navigating to nc will throw an exception (Unable to parse URI)
		- if this happens, a restart of the avatar docker-compose will be required


+ docker-desktop
Install docker desktop
Install WSL 2
	* Step 4 - Download the Linux kernel update package
		* https://docs.microsoft.com/en-us/windows/wsl/install-manual
		* https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
		

+ cmd
	- docker-compose -f docker-compose.yml up
+ update avatar-server-pack/traefik.toml
	- traefik restart not required
	- add frontend and backend to redirect to avatar
+ cortex-graph-in
	- invoke regenerate to initialize db
+ browser
	- navigate to nCode
		- http://[traefik-ip]:[traefik-port]/[avatar]/nc/tree
	- set in "avatar url" bar
		- http://[traefik-ip]:[traefik-port]/[avatar]/cortex/neurons?sortorder=0
		! no need to reload avatar
	+ create [owner neuron]
+ send data to neurul server admin to add to config
	- idp
		- d23-variables.env\client_id + secret
			! update Clients
				- ClientName
				- RedirectUris
				- PostLogoutRedirectUris
		- variables.env\api_name + secret
			! update ApiResources
				- DisplayName		
	- reverse-proxy
		- update
		! remember to include docker-compose.override.yml in docker-compose up command
+ update identity-access.db
	- User table
		- add record
			- userid
				- google email address or subjectid (if using built in credentials) of admin
			- neuronid
				- [owner neuron] guid
	- Region Permit
		- add record
			- userneuronid
				- [owner neuron] guid
			- regionneuronid
				- NULL if base region
			- In the WriteLevel column, enter:
				- "0" - no write access
				- "1" - write to neurons created by the user only
				- "2" - write to neurons created by all users
			- In the CanRead column, enter:
				- "0" - no read access
				- "1" - read neurons
- Sign-in using owner neuron in nCode
	- Create "Guest" neuron
	- Add to identity-access
		- user
			- UserId
				- random guid
			- NeuronId
				- guid id of neuron created in previous step
	- update avatar-server-pack\variables.env\anonymous_user_id
		- randomly generated guid, linked to an created [anonymous neuron] via User table in identity-access.db
		- normally created as 2nd neuron (after [owner neuron])
	- restart avatar to reload anonymous_user_id
