<!-- mdtocstart -->

# Table of Contents

- [Static Analysis](#static-analysis)
- [Testing](#testing)
    - [Network Failure Injection](#network-failure-injection)
    - [Mocking](#mocking)
- [Debuggers](#debuggers)
    - [Memory](#memory)
    - [Python](#python)
- [Profiling](#profiling)
    - [Memory](#memory-1)
    - [CPU](#cpu)
    - [Network](#network)
    - [Disk](#disk)
    - [DBus](#dbus)
    - [C/C++](#cc)
- [Hacking Tools](#hacking-tools)
- [Network](#network-1)
    - [Scanners](#scanners)
    - [Diagnostic / Debug](#diagnostic--debug)
- [Databases](#databases)
    - [SQL](#sql)
    - [Time Series](#time-series)
    - [Migration](#migration)
    - [Schema visualization](#schema-visualization)
    - [Data visualization](#data-visualization)
- [Message Brokers](#message-brokers)
- [Monitoring](#monitoring)
- [Presentations](#presentations)
- [Software Defined Network](#software-defined-network)
- [Performance/Load tests](#performanceload-tests)
- [Web](#web)
    - [Browser Automation](#browser-automation)
    - [Secret Management](#secret-management)
- [Diagrams](#diagrams)
    - [Sequence](#sequence)
    - [Graphs](#graphs)
- [HTTP traffic analysis](#http-traffic-analysis)
- [Scrapping](#scrapping)
    - [Parsing](#parsing)
    - [Services](#services)
- [Proxy Providers](#proxy-providers)
    - [Dedicated/Shared IP Proxies](#dedicatedshared-ip-proxies)
        - [User/Pass Authentication](#userpass-authentication)
        - [IP Authentication](#ip-authentication)
    - [BackConnect](#backconnect)
- [Captcha Breaking](#captcha-breaking)
- [Email Verification](#email-verification)
- [Distributed System Validation](#distributed-system-validation)
- [Security Protocol Validation](#security-protocol-validation)
- [WASM](#wasm)
    - [Profiling](#profiling-1)

<!-- mdtocend -->

# Static Analysis

* [codeql](https://codeql.github.com/)
* [semgrep](https://semgrep.dev/) 


# Testing

## Local Cloud Mocking

* [localstack](https://github.com/localstack/localstack)


## Network Failure Injection

* [comcast](https://github.com/tylertreat/comcast)


## Mocking

* [Mountebank](http://www.mbtest.org/)
* [Toxiproxy](https://github.com/shopify/toxiproxy)
* [Hamms](https://github.com/kevinburke/hamms)
* [HoverFly](https://github.com/SpectoLabs/hoverfly)
* [VCR](https://github.com/vcr/vcr)
* [gor](https://github.com/buger/gor)
* [Pacto](http://thoughtworks.github.io/pacto/)
* [Pact](https://github.com/realestate-com-au/pact)


# Debuggers

* [Fenris](https://lcamtuf.coredump.cx/fenris/whatis.shtml)

## Memory

* [Dmalloc](http://dmalloc.com/)
* [Efence](http://elinux.org/Electric_Fence)
* [Valgrind](http://valgrind.org/)
* [memwatch](http://www.linkdata.se/sourcecode/memwatch/)
* [mtrace](http://www.gnu.org/software/libc/manual/html_node/Tracing-malloc.html)

## Python

* [Heapy](http://guppy-pe.sourceforge.net/)


# Profiling

* [perf-tools](https://github.com/brendangregg/perf-tools)
* [Perf](https://perf.wiki.kernel.org/index.php/Main_Page)
* [ftrace](https://www.kernel.org/doc/Documentation/trace/ftrace.txt)
* [envdb](https://github.com/mephux/envdb)
* [OProfile](http://oprofile.sourceforge.net/about/)
* [KCacheGrind](http://kcachegrind.sourceforge.net/html/Home.html)
* [dstat](http://linux.die.net/man/1/dstat)


## Memory

* [vmstat](http://linux.die.net/man/8/vmstat)


## CPU

* [mpstat](http://linuxcommand.org/man_pages/mpstat1.html)
* [cpustat](https://github.com/uber-common/cpustat)


## Network

* [ntop](http://www.ntop.org)
* [Nethogs](http://nethogs.sourceforge.net/)
* [iftop](http://www.ex-parrot.com/~pdw/iftop/)
* [iptraf](http://iptraf.seul.org/)
* [Iperf](https://github.com/esnet/iperf)
* [qperf](http://linux.die.net/man/1/qperf)


## Disk

* [fio](http://linux.die.net/man/1/fio)
* [ioping](https://code.google.com/p/ioping/)
* [iotop](http://linux.die.net/man/1/iotop)
* [hdparm](http://man7.org/linux/man-pages/man8/hdparm.8.html)


## DBus

* [Bustle](http://www.willthompson.co.uk/bustle/)


## C/C++

* [gperf](https://code.google.com/p/gperftools/)
* [gprof](http://www.cs.utah.edu/dept/old/texinfo/as/gprof.html)


# Hacking Tools

* [Collection of tools](http://gexos.github.io/Hacking-Tools-Repository/)


# Network 

* [Cacti](http://www.cacti.net/)
* [MRTG](http://oss.oetiker.ch/mrtg/)
* [Netem](http://www.linuxfoundation.org/collaborate/workgroups/networking/netem)
* [T50 Packet Injector](http://t50.sourceforge.net/index.html)
* [CORE](http://www.nrl.navy.mil/itd/ncs/products/core)
* [keepalived](http://www.keepalived.org/)
* [Hydra](https://www.thc.org/thc-hydra/)


## Scanners

* [nmap](http://nmap.org/)
* [masscan](https://github.com/robertdavidgraham/masscan)


## Diagnostic / Debug

* [mtr](http://linux.about.com/library/cmd/blcmdl8_mtr.htm)
* [netstat](http://linux.die.net/man/8/netstat)
* [tcpdump](http://www.tcpdump.org/)
* [tshark](https://www.wireshark.org/docs/man-pages/tshark.html)


# Databases

## SQL

* [cockroach](https://github.com/cockroachdb/cockroach)
* [citus](https://github.com/citusdata/citus)
* [timescaledb](https://github.com/timescale/timescaledb)


## Time Series

* [M3](https://github.com/m3db/m3)
* [OpenTSDB](http://opentsdb.net/)


## Migration

* [Flyway](https://github.com/flyway/flyway)


## Schema visualization

* [SchemaSpy](http://schemaspy.sourceforge.net/)


## Data visualization

* [superset](https://github.com/apache/incubator-superset)


# Message Brokers

* [Kafka](http://kafka.apache.org/)
* [RabbitMQ](http://www.rabbitmq.com/)
* [0MQ](http://zeromq.org/)
* [NSQ](https://github.com/bitly/nsq)


# Monitoring

* [SysDig](http://www.sysdig.org/)
* [Prometheus](http://prometheus.io/)
* [Zabbix](http://www.zabbix.com/)
* [Monit](https://mmonit.com/monit/)
* [Munin](http://munin-monitoring.org/)
* [collectd](https://collectd.org/)
* [Performance Co Pilot](http://pcp.io/)
* [cAdvisor](https://github.com/google/cadvisor)
* [Sensu](https://github.com/sensu/sensu)
* [Grafana](http://grafana.org/)


# Presentations

* [Go Present](https://godoc.org/golang.org/x/tools/cmd/present)
* [sli.dev](https://sli.dev/)
* [mdp](https://github.com/visit1985/mdp)
* [reveal.js](http://lab.hakim.se/reveal-js)
* [dzslides](http://paulrouget.com/dzslides/)


# Software Defined Network

* [Calico](http://www.projectcalico.org/)
* [Weave](https://github.com/zettio/weave)
* [Flannel](https://github.com/coreos/flannel)
* [Pipework](https://github.com/jpetazzo/pipework)


# Performance/Load tests

* [k6](https://k6.io/)
* [Gatling](http://gatling-tool.org/)
* [httperf](http://www.hpl.hp.com/research/linux/httperf/)
* [ab](http://httpd.apache.org/docs/2.2/programs/ab.html)
* [locust](https://locust.io/)
* [Bees with machine guns](https://github.com/newsapps/beeswithmachineguns)
* [JMeter](http://jmeter.apache.org/)


# Web

## Desktop

* [Tauri](https://github.com/tauri-apps/tauri)
* [Wails](https://github.com/wailsapp/wails)


## Browser Automation

* [Puppeteer](https://developers.google.com/web/tools/puppeteer)
* [Selenium](http://www.seleniumhq.org/)
* [PhantomJS](http://phantomjs.org/)
* [chromedp](https://github.com/chromedp/chromedp)
* [marionette](https://firefox-source-docs.mozilla.org/testing/marionette/marionette/index.html)


## Secret Management

* [Vault](https://vaultproject.io/)
* [Keywhiz](https://square.github.io/keywhiz/)


# Diagrams

* [diagrams.net](https://www.diagrams.net/)
* [excalidraw](https://github.com/excalidraw/excalidraw)
* [diagrams](https://github.com/mingrammer/diagrams)
* [Dia](https://wiki.gnome.org/Apps/Dia/)
* [IcePanel](https://icepanel.io/)
* [Inkscape](http://inkscape.org/en/)
* [ditaa](http://ditaa.sourceforge.net/)


## Text to Diagram

* [mermaid-js](https://github.com/mermaid-js/mermaid)
* [d2](https://github.com/terrastruct/d2)
* [Graphviz](https://graphviz.gitlab.io/)


## Sequence

* [MscGen](https://en.wikipedia.org/wiki/MscGen)
* [MscLatex](http://satoss.uni.lu/software/mscpackage)
* [Sequence Diagrams](https://www.websequencediagrams.com/)


## Graphs

* [Gephi](http://gephi.github.io/)


# HTTP traffic analysis

* [mitm](https://github.com/mitmproxy/mitmproxy)
* [wuzz](https://github.com/asciimoo/wuzz)


# Scrapping 


## Parsing

* [scrapely](https://github.com/scrapy/scrapely)
* [parsley](https://github.com/fizx/parsley)


## Services

* [import.io](https://import.io)
* [kimonolabs](https://www.kimonolabs.com/)
* [scrapinghub](http://scrapinghub.com/)
* [cloud scrape](http://cloudscrape.com/)
* [diffbot](https://www.diffbot.com/)
* [dexi.io](https://dexi.io/)
* [diggernaut](https://www.diggernaut.com/dev/meta-language-runtime-entities.html)


# Proxy Providers

## Dedicated/Shared IP Proxies

### User/Pass Authentication

* [storm proxies](http://stormproxies.com/)
* [buyproxies](https://buyproxies.org/)
* [proxyvoxy](https://proxyvoxy.com/)
* [microleaves](https://microleaves.com)


### IP Authentication

* [ezproxies](https://www.ezproxies.com/)
* [shared proxies](http://www.sharedproxies.com/)
* [instantproxies](https://instantproxies.com/)


## BackConnect

* [luminati.io](https://luminati.io/)
* [proxymesh](http://proxymesh.com/)


# Captcha Breaking

* [2 captcha](https://2captcha.com/)
* [Death by captcha](http://www.deathbycaptcha.com/)
* [Image Typerz](http://www.imagetyperz.com/)
* [Pixodrom](http://pixodrom.com/)


# Email Verification

* [Verify Email](http://verify-email.org/register/levels.html)


# Distributed System Validation

* [jepsen](https://github.com/jepsen-io/jepsen)
* [TLA+](http://research.microsoft.com/en-us/um/people/lamport/tla/tla.html)
* [Runway](https://github.com/salesforce/runway-browser)

# Security Protocol Validation

* [tamarin](https://tamarin-prover.github.io/)

# WASM

* [binaryen](https://github.com/WebAssembly/binaryen)

## Profiling

* [twiggy](https://github.com/rustwasm/twiggy)
