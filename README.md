#Service Mashup - Microservices in Praxis

##Ziele

* Schnell, Günstig, Sicher

##Organisation und Prozess

* Continuous Improvement
* Agile Architecture

##Analyse, Design & Architektur

1. Welche Verticals haben war? Wie wird verteilt? -> Schnitt von Mircoservices (Mike)
  * Entscheidungsfindung (Timebox) <- sonnst Refactoring
  * Wie nicht weiß wie sinnvoll geschnitten sollte (z.B. Domänenwissen fehlt) monolithisch beginnen. ACHTUNG Timebox einhalten.

##Best Practive
1. Unterschiede operative Daten und Report- / Historie- / Stamm- Daten (Mike)
  * Microservice verwenden ausschließlich eigene operative Daten (Datenhoheit - kein shared state)
2. Problemmanagement und Kommunikation (Andi)

##ECommerce Beispiel

![Architektur]() (Mike)
![E-Commerce UI]() (Andi)
![Sales UI]() (Andi)
![Infrastructure]() (Mike)
![Accounting UI]()

##Stabilität und Verfügbarkeit

Mike = 5,4,6
Andi = 1,2,3

1. Discovery & Fallback (immer min. 2 Instanzen)
  * URL Fallback im Client (Code)
  * URL Fallback im Backend (Code)

2. Statefull & Integration (Orchestration) UI (Code)  
  * Atom Writes (Code)
  * https://github.com/ServiceMashup/web-app/blob/master/app/cart.directive.js

3. Data Replication (Code)
  * Replicate data from Data Warehouse 

4. Webhooks (Code) https://github.com/ServiceMashup/contract-service
  * Replicate Data to Data Warehouse

5. Kommunikation einer möglich Fehlerkompensation am Beispiel vom Contract Service klar machen (Code)
  * Sprechen mit Product-Owner und abwägen von Risiken, Kosten und Nutzen
  * Graceful Shutdown - z.B. drainen von nodes

6. Scale out - Workload (Load-Balancer kompensation)
  * Round-Robin on Consumer Side ???
  * Rate limits ???

##Infrastruktur

(aside) ??? 

* Deployment (Docker, Shipyard)
* Discovery (Consul.io)
* Routing (Proxy)
* Monitoring / SLA (Log-Collector via FluentD, Elastic Search - Kibana - LogStash)