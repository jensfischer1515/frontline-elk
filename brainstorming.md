# Log management @frontline

## about frontline

 * Streetfashion onlineshop
 * since X
 * 
 
## motivation
 * no logfile access
 * Dev != Ops
 
### History

 * kein Zugriff
  * shell zugriff? read only zugriff?
  * per cron transportieren?
 
 * angefangen lokal mit alten internen ERP Log dateien
 * Anfang 2013 Logstash 1.1...?auf Live
  * ES 0.9x
  * Kibana 2
 * zu viele Log-Dateien
 * unstrukturiert
 * grep, awk?
 * deploy log format updates? -> staging ELK
 
### Life of a Logevent
 * create (apache access log, java logback)
 * transport (logstash push to redis)
 * analyze/filter/store? (logstash pop from redis, filter log event and push to elasticsearch)
 * backup (daily index, ES snapshots with curator)
 * delete (clear old snapshots, clear old indcies with curator)
  
## ELK

 * Howto scale ELK?

 * **E** lasticsearch
 * **L** ogstash
 * **K** ibana
 

## ELK setup

 **image** without broker
 **image** with broker, entkopplung, queue, sipper performance
 
# Logstash

 * input -> filter -> output
 
## filter probleme?
### apache example
### logback example
 * stacktrace
 * xml
 * orders

#### multiline filter
 * stacktrace
 * xml
 
#### grok filter
 ```
 "%{TIME:timestamp} \[%{DATA:thread}\] \[\#:(?:%{DATA:exHash})\] \[(?:%{DATA:sessionid})@(?:%{IPV4:ip}?)\] \[(?:%{DATA:rnd})\/(?:%{DATA:username})\] %{LOGLEVEL:level}\s*%{JAVACLASS:marker} - (?<logmessage>(?:.|\n\t)*)"
 ```


## filter lösungen
 * Elasticsearch? -> log in JSON
 * Apache LogFormat
 * logback-logstash-encoder
 
 
## Logback Beispiel
 * orders
 
 ```
 class OrderData {
 	List<LineItem> lineItems;
	Money totalAmount;
	PaymentType paymentType;
	ShipperType shipperType; 
 }
 ```
 
 ```
 Logger.info(orderData);
 ```
 
 ```
 {
 	"lineItems": [{}, {}, ...],
 	"totalAmount": 23.55,
 	"paymentType": "PAYPAL",
 	"shipperType": "DHL"
 }
 ```