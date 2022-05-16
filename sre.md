# SRE
## SLI, SLO, SLA
### SLI (Service Layer Indicator)
_What should be true? Our median request latency is less than 500ms._

It is a defined quantitative measure of some aspect of service that is provided and focused on customer experience within a moment of time.
    1. SLIs: latency, RPS, percentiles etc.
    2. Non-SLIs: CPU utilization, network and memory usage etc
    
### SLO (Service Layer Objective)
_What proportion of time? We achieve our SLI 99.9% of the time._ 

It is a target value or range of values for a service which are measured by SLI - that is how often or how long we want to achive specified goal. F.e., latency <= 10s, yearly uptime <= 99.99%

### SLA (Service Layer Agreement)
_What happens if we don't achieve that? We compensate the customer with credits if we don't meet our SLA._

It is an explicit contract between your users and usually per product, that includes consequences of meetings (or missings) of SLOs of the services. This the agreement that put expectations for customers. That is if you say your service is available for 99.95% uptime, customer expect that availability for their services.

## LCE - Launch Coordination Engineer
LCE consists of SRE engineers from all related products and have most experience.
Their first responsibility is to provide appropriate domain knowledge before launching
the product. The domain knowledge consists from checklist that includes launch requirements:
- compliance with company standards and best practices, provide actions to improve reliability
- connecting and synchronizing the teams from related products
- driving technical aspects
- educate developers with best practices, guide to integrate or reuse existing services if
  exists, provide appropriate documentation and internal training resources

## Dashboards
Some practices to keep metrics dashboard clean and understandable:
- focus on iconic and short-term memory, meaning ability for human to save image and process within limited 
  fraction of time
- use colors and shapes to distinguish various information by its priority. Keep in mind, colored graphs best
  for categorization, whereas shapes best for quantitive data  
- use groups to gather related metrics
- if there is a link to another dashboard - there must be consistency in linking
- apply 2 observability strategies correctly:
  1. USE (Utilization, Saturation and Errors) - used to estimate "how happy your machines or infrastructure are"
     and applied to hardware and underlying infrastructure.
  2. RED (Rates, Errors and Durations) - used to estimate "how happy customers are". Used in application 
     services/microservices environments to identify customer impacts which most important for the business. 
     Most of the time monitors are created based on RED strategy not USE.
- The Four Golden Signals (Google SRE handbook) - the same as RED but with saturation, where saturation defines - 
  "how full the system is".

There is my `docker-compose` repository [here](https://github.com/sabyrzhan/docker), where you can 
find `docker-compose-grafana.yml` file. I used Prometheus, Grafana, Graphite, cAdvisor, Prometheus reporters and 
collectd to build the dashboard based on host metrics. Almost all dashboards I imported from community dashboards 
just to see how they are used and configured.
