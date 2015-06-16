# elk-stack

> Dockerized ELK stack: Elasticsearch, Logstash and Kibana.

Images used to build the stack:

* [elasticsearch](https://registry.hub.docker.com/_/elasticsearch/)
* [logstash](https://registry.hub.docker.com/u/library/logstash/)
* [kibana](https://registry.hub.docker.com/u/pasangsherpa/kibana/)

Checkout [logstash-forwarder image](https://registry.hub.docker.com/u/pasangsherpa/logstash-forwarder/) to setup logstash-forwarder.

## Installation and Setup

1. First, Install [Docker and Compose][1] **OR** Install [Vagrant][6] and [Virtualbox][7].

2. Next, clone the project.

	```
	$ git clone git@github.com:pasangsherpa/elk-stack.git
	$ cd elk-stack
	```
	
3. Skip this step if you didn't install or don't want to use [Vagrant][6].
	
	```
	$ vagrant up
	$ vagrant ssh
	$ cd /vagrant
	```

## Configure 

1. [Elasticsearch][2] configuration files can be found under 'config/elasticsearch' folder. The folder comes with two files, the elasticsearch.yml for configuring [Elasticsearch][2] different modules, and logging.yml for configuring the [Elasticsearch][2] logging. Update these configuration files as per your need or leave it as it is. For more info checkout [Elasticsearch-setup-configuration](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-configuration.html).

2. [Logstash][3] configuration file can be found under 'config/logstash/conf' folder. This is where you will define logstash inputs, filters and outputs. Default config includes setup for [logstash-forwarder][5].

3. [Kibana][4] configuration file can be found under 'config/kibana' folder.

4. **[Optional]** Setup logstash-forwarder
	1. 	Generate ssl certificate for logstash server. *Note: This step is only required if you decide to use [logstash-forwarder][5].*
	
		```
		$ cd config/logstash/tls
	
		# NOTE: replace <logstash_server_fqdn> with your logstash server dns.
		$ openssl req -subj '/CN=<logstash_server_fqdn>/' -x509 -days 3650 -batch -nodes -newkey rsa:2048 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt
		```
	2. Copy the generated certs from logstash server to 'ssl/certs/logstassh-forwarder.crt' in [logstash-forwarder] [5] server.
	3. Add the name of the logstash server to logstash-forwarder.conf file in [logstash-forwarder] [5] server.

## Build and run ELK with Compose

1. Build and run [elasticsearch][2], [logstash][3] and [kibana][4] container.

	```
	$ docker-compose up -d // Run all service at once (-d flag runs container in daemon mode)
	$ docker-compose up // Run all service at once
	
	// Run one service at a time in background (-d flag runs container in daemon mode)
	$ docker-compose up -d elasticsearch
	$ docker-compose up -d logstash
	$ docker-compose up -d kibana

	```

2. Build and run logstash container as [logstash][3] executable.

	```
	// NOTE: docker-compose run command will not expose ports, you need to run docker-compose up logstash first. 
	// https://github.com/docker/compose/issues/1256#issuecomment-90135857
	$ docker-compose run logstash -h
	```

3. To stop all services

	```
	$ docker-compose stop
	```
	

## Verify
1. Go to [http://localhost:9200](http://localhost:9200) to verify elasticsearch is running.
2. Go to [http://localhost:5601](http://localhost:5601) to verify kibana is running.


## License

[MIT](http://opensource.org/licenses/MIT) © [Pasang Sherpa](https://github.com/pasangsherpa)

[1]: https://docs.docker.com/compose/install/
[2]: https://www.elastic.co/products/elasticsearch
[3]: https://www.elastic.co/products/logstash
[4]: https://www.elastic.co/products/kibana
[5]: https://www.elastic.co/guide/en/logstash/current/plugins-inputs-lumberjack.html
[6]: http://www.vagrantup.com/downloads.html
[7]: https://www.virtualbox.org/wiki/Downloads