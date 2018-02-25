# ElasticSuricata
Example Configurations for Elastic + Suricata EVE

## Requirements
* Elastic 6
* Logstash 6
* Recent version of redis (default configuration fine if also running localhost) 
* Suricata with EVE enabled (ideally use pfSense for a rapid install / setup ) - configure to send EVE data to your REDIS server

## Solution Diagram

Suricata --->  EVE REDIS Data ---> REDIS ---> Logstash REDIS Client ---> Logstash ---> HTTP(s) --> ElasticSearch

## Important Notes / Warnings

If you choose to use reverse DNS lookups as added fields - use caution (or comment those out!) 

1) Use a local DNS sever that mirrors root servers daily - notice our configurations are localhost to further improve lookup speeds. 
2) Lookup on some DNS IP's / domains will trigger IDS/IPS rules, so use caution again if you choose to reverse lookup IP's

If you haven't got the hint yet, comment out the entire `mutate` and `dns` JSON elements if just getting started. Mess with those at a later time.

* Copy template to templates folder - for example  `/etc/logstash/templates/suricata_template.json`
* Copy logstash configution to conf.d folder - for example `/etc/logstash/conf.d/logstash_suricata_eve.conf`

## Replica and Shards

* Note that the profiled mapping templae sets replica count to 0 and shards to 1. This is ideal for a home lab using a single elasticsearch node. But you will want to adjust to suit any production or larger scale cluster / data footprint. 

## Disclaimers and Warnings

The above configuration files are for Elastic 6.x and will not work with 5.x or earlier versions. 

It most certainly is not an optimal configuration - just 'enough' to get you up and running. If you choose to send the FULL set of EVE Data including flows in a larger scale network enviroment, there will be alot of indexing occuring. Fine for home, but can pinch harder at work.

No warrenties expressed or implied. Use at your own risk.  Happy to get PR's with enhancements, fixes or suggestions on better mapping configurations or indexing optimizations. 
