## 15/03/15 Skype

* PDF Fallback: http://localhost:8000/?print-pdf#/
* (JF) Metriken: Cluster, Dokumente
* (JF) Live Kibana (NASA access logs)

* Screenshot apache logs
* Screenshot stacktraces
* Foto Dashboard TFT
* Logvolumen?
 * Frontline Zahlen?

## 13/03/15 Treffen

 * MDC Felder
 * unique request id
 * CSV Expoert Kibana 4
 
### TODOs Timo
 * ApacheLogFormat suchen!
 * logback.xml encoder config
 * Stacktrace Dashboard
 * Logvolumen?
 * How to Scale

### TODO Jens
 * logfile aufteilung / fachliche trennung?
 * type field / redis key name / logfile-Name conventions?
 * ishop wire? logback-sifftingappender
 * performance (`Doc Value`)
 * Benchmarks (Logvolumen?)
 * logback-logstash-appender AsyncDisruptorAppender
 https://github.com/logstash/logstash-logback-encoder#usage

### Ausblick

 * ES Templates
   * Datenvolumen begrenzen (kein `_all` Feld)
   * `.raw` Felder
   * *not analysed* Steuerung der Felder
   * filter cache
 * Monitoring
   * Metriken
 * Skalierung
 * Kibana 4


### weitere learnings
 * ES: Queries vs. Filter
   * aggregation by query
   * -> Kibana 4
 * Staging Setup um Konfigurationsänderungen zu testen
 * redis died

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
 
## filter

* typische webserver log zeile 
   * typisches parsen mit filtern
   * `${COMBINEDAPACHELOG}`
   * `FROM PATTERN`
   * -> erzeugt:
      * rubydebug beispiel
   * kibana dashboard
   * timestamps log event vs. index zeit
   
 * app log example
   * timestamp
   * loglevel
   * class/method
   * bspw. ishop log zeile
   * patterns und tags und conditions
   * default ishop log line
   * stacktace und xml oder so (multiline)

 
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




 ## Apache Logfiles
* http://ita.ee.lbl.gov/html/traces.html
* http://ita.ee.lbl.gov/html/contrib/NASA-HTTP.html
