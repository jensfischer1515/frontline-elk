# DEMO CLIPBOARD

## Simple Logstash Demo

echo fooBar | ./bin/logstash -f simple.conf


## Apache Access Log Demo

### Apache Access Logs laden

	wget ftp://ita.ee.lbl.gov/traces/NASA_access_log_Jul95.gz
	gunzip NASA_access_log_Jul95.gz


### Elasticsearch starten

	wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.4.4.tar.gz
	tar xvzf elasticsearch-1.4.4.tar.gz
	cd elasticsearch-1.4.4
	./bin/elasticsearch --http.cors.enabled=true --http.cors.allow-origin=http://localhost:8000 &
	cd ..
	open http://localhost:9200/_status?pretty


### Logs durch Logstash jagen

	wget https://download.elasticsearch.org/logstash/logstash/logstash-1.4.2.tar.gz
	tar xvzf logstash-1.4.2.tar.gz
	cd logstash-1.4.2
	cat ../access_log_Jul95 | ./bin/logstash -f apache_elasticsearch.conf
	cd ..


### Kibana 3 starten
	
	wget https://download.elasticsearch.org/kibana/kibana/kibana-3.1.2.tar.gz
	tar xvzf kibana-3.1.2.tar.gz
	cd kibana-3.1.2
	npm install -g local-web-server
	ws &
	open http://localhost:8000


### Kibana 4 starten

	wget https://download.elasticsearch.org/kibana/kibana/kibana-4.0.1-darwin-x64.tar.gz
	tar xvzf kibana-4.0.1-darwin-x64.tar.gz
    cd kibana-4.0.1-darwin-x64
    ./bin/kibana &
    cd ..
	open http://localhost:5601
