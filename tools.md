<!-- mdtocstart -->

# Table of Contents

- [Testing](#testing)
    - [Chaos](#chaos)
    - [Networking](#networking)
    - [Management/Planning](#managementplanning)
    - [Mocking](#mocking)
- [Debuggers](#debuggers)
    - [Go](#go)
    - [Memory](#memory)
        - [Python](#python)
- [Continuous Delivery](#continuous-delivery)
    - [On premisses](#on-premisses)
    - [Services](#services)
- [Static Analysis](#static-analysis)
    - [C / C++](#c--c)
    - [Python](#python-1)
    - [Golang](#golang)
- [Profiling](#profiling)
    - [Distributed System Tracing](#distributed-system-tracing)
    - [Memory](#memory-1)
    - [CPU](#cpu)
    - [Network](#network)
    - [Disk](#disk)
    - [Golang](#golang-1)
    - [MongoDB](#mongodb)
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
    - [Benchmarks](#benchmarks)
- [Logging](#logging)
    - [Aggregate](#aggregate)
    - [Analytics](#analytics)
- [Message Brokers](#message-brokers)
- [Project Mangement](#project-mangement)
- [Monitoring](#monitoring)
- [Presentations](#presentations)
- [Software Defined Network](#software-defined-network)
- [Penetration Tests](#penetration-tests)
- [Performance/Load tests](#performanceload-tests)
- [Web](#web)
    - [Browser Automation](#browser-automation)
    - [Secret Management](#secret-management)
- [Diagrams](#diagrams)
    - [Sequence](#sequence)
    - [Graphs](#graphs)
- [HTTP Reverse Proxies / Load balancers](#http-reverse-proxies--load-balancers)
- [HTTP traffic analysis](#http-traffic-analysis)
- [Disk Recovery](#disk-recovery)
- [Speech Analysis](#speech-analysis)
- [OCR](#ocr)
- [Image Tagging](#image-tagging)
- [Deep Learning](#deep-learning)
- [Content Analysis](#content-analysis)
- [PDF](#pdf)
- [Natural Language Processing](#natural-language-processing)
- [Scrapping](#scrapping)
    - [Parsing](#parsing)
    - [Services](#services-1)
- [Proxy Providers](#proxy-providers)
    - [Dedicated/Shared IP Proxies](#dedicatedshared-ip-proxies)
        - [User/Pass Authentication](#userpass-authentication)
        - [IP Authentication](#ip-authentication)
    - [BackConnect](#backconnect)
- [Captcha Breaking](#captcha-breaking)
- [Email Verification](#email-verification)
- [Distributed System Validation](#distributed-system-validation)

<!-- mdtocend -->

# Testing

## Chaos

* [pumba](https://github.com/gaia-adm/pumba)
* [kube-monkey](https://github.com/asobti/kube-monkey)


## Networking

* [comcast](https://github.com/tylertreat/comcast)


## Management/Planning

* [Testopia](https://developer.mozilla.org/en-US/docs/Mozilla/Bugzilla/Testopia)
* [GTA](https://code.google.com/p/test-analytics/)
* [Tarantula](http://www.testiatarantula.com/)


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

## Go

* [delve](https://github.com/derekparker/delve)


## Memory

* [Dmalloc](http://dmalloc.com/)
* [Efence](http://elinux.org/Electric_Fence)
* [Valgrind](http://valgrind.org/)
* [memwatch](http://www.linkdata.se/sourcecode/memwatch/)
* [mtrace](http://www.gnu.org/software/libc/manual/html_node/Tracing-malloc.html)


### Python

* [Heapy](http://guppy-pe.sourceforge.net/)


# Continuous Delivery

## On premisses

* [Spinnaker](https://github.com/spinnaker/spinnaker)
* [Jenkins](http://jenkins-ci.org/)
* [Go](http://www.go.cd/)
* [Gitlab CI](https://about.gitlab.com/gitlab-ci/)
* [Buildbot](http://buildbot.net/)
* [Helios](https://github.com/spotify/helios)


## Services

* [Snap](https://snap-ci.com/)
* [Strider CD](http://stridercd.com/)
* [Travis](https://travis-ci.org/)
* [Coveralls](https://coveralls.io/)
* [CircleCI](https://circleci.com/)


# Static Analysis

* [Simian](http://www.harukizaemon.com/simian/)
* [PMD](http://pmd.sourceforge.net/)
* [CCFinderX](http://www.ccfinder.net/ccfinderxos.html)
* [Clone Digger](http://www.ccfinder.net/ccfinderxos.html)
* [SonarQube](http://www.sonarqube.org/)
* [oclint](http://oclint.org/)
* [cccc](http://cccc.sourceforge.net/)
* [Coverity](http://www.coverity.com/)
* [Linthub](https://linthub.io/)


## C / C++

* [SonarQube C++](https://github.com/wenns/sonar-cxx)
* [cppcheck](http://cppcheck.sourceforge.net)
* [splint](http://splint.org/)


## Python

* [pyflakes](https://github.com/pyflakes/pyflakes/)
* [pylint](http://www.pylint.org/)
* [pep8](https://github.com/jcrocholl/pep8)


## Golang

* [go-vet](https://golang.org/cmd/vet)
* [go-unused](https://github.com/dominikh/go-unused)
* [go-simple](https://github.com/dominikh/go-simple)
* [go-staticcheck](https://github.com/dominikh/go-staticcheck)
* [gometalinter](https://github.com/alecthomas/gometalinter)
* [dupl](https://github.com/mibk/dupl)
* [goviz](https://github.com/hirokidaichi/goviz)


# Profiling

* [perf-tools](https://github.com/brendangregg/perf-tools)
* [Perf](https://perf.wiki.kernel.org/index.php/Main_Page)
* [ftrace](https://www.kernel.org/doc/Documentation/trace/ftrace.txt)
* [Iperf](https://github.com/esnet/iperf)
* [DTrace](http://dtrace.org/blogs/)
* [System tap](https://sourceware.org/systemtap/)
* [osquery](https://github.com/facebook/osquery)
* [envdb](https://github.com/mephux/envdb)
* [OProfile](http://oprofile.sourceforge.net/about/)
* [KCacheGrind](http://kcachegrind.sourceforge.net/html/Home.html)
* [dstat](http://linux.die.net/man/1/dstat)


## Distributed System Tracing

* [zipkin](http://zipkin.io/)


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
* [qperf](http://linux.die.net/man/1/qperf)


## Disk

* [fio](http://linux.die.net/man/1/fio)
* [ioping](https://code.google.com/p/ioping/)
* [iotop](http://linux.die.net/man/1/iotop)
* [hdparm](http://man7.org/linux/man-pages/man8/hdparm.8.html)


## Golang

* [go-torch](https://github.com/uber/go-torch)


## MongoDB

* [mtools](https://github.com/rueckstiess/mtools)


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

* [cockroach](https://github.com/cockroachdb/cockroach)
* [arangodb](https://www.arangodb.com/)
* [blinkdb](http://blinkdb.org/)
* [druid](http://druid.io/)
* [pipelinedb](https://www.pipelinedb.com/)
* [ceph](http://ceph.com/)
* [Presto](http://prestodb.io/)
* [InfluxDB](http://influxdb.com/)
* [Scylla](https://github.com/scylladb/scylla)
* [Camlistore](http://camlistore.org/)
* [RethinkDB](http://www.rethinkdb.com/)
* [ZeroDB](https://www.zerodb.io/)


## SQL

* [citus](https://github.com/citusdata/citus)
* [timescaledb](https://github.com/timescale/timescaledb)


## Time Series

* [OpenTSDB](http://opentsdb.net/)


## Migration

* [Flyway](https://github.com/flyway/flyway)


## Schema visualization

* [SchemaSpy](http://schemaspy.sourceforge.net/)


## Data visualization

* [superset](https://github.com/airbnb/superset)


## Benchmarks

* [hammerdb](http://www.hammerdb.com/)


# Logging

* [log.io](http://logio.org/)
* [Bosun](http://bosun.org/)
* [Chukwa](http://chukwa.apache.org/)
* [Riemann](http://riemann.io/)
* [Flume](http://flume.apache.org/)
* [Gollum](https://github.com/trivago/gollum)
* [Logspout](https://github.com/gliderlabs/logspout)
* [Ekanite](https://github.com/ekanite/ekanite)
* [Kibana](https://www.elastic.co/products/kibana)
* [RRDtool](http://oss.oetiker.ch/rrdtool/)


## Aggregate

* [Logstash](http://www.logstash.net/)
* [Graylog2](http://graylog2.org/)
* [Heka](https://github.com/mozilla-services/heka)
* [FluentD](http://www.fluentd.org/)


## Analytics

* [Sentry](https://www.getsentry.com/welcome/)
* [Piwik](http://piwik.org/)
* [SnowPlow](http://snowplowanalytics.com/)
* [Inviso](https://github.com/Netflix/inviso)


# Message Brokers

* [Kafka](http://kafka.apache.org/)
* [jocko](https://github.com/travisjeffery/jocko)
* [RabbitMQ](http://www.rabbitmq.com/)
* [0MQ](http://zeromq.org/)
* [Iris](http://iris.karalabe.com/)
* [NSQ](https://github.com/bitly/nsq)
* [cherami](https://github.com/uber/cherami-server)
* [Celery](http://www.celeryproject.org/)
* [Siberite](https://github.com/bogdanovich/siberite)
* [nchan](https://github.com/slact/nchan)


# Project Mangement

# Monitoring

* [SysDig](http://www.sysdig.org/)
* [sysdig-inspect](https://github.com/draios/sysdig-inspect/)
* [Prometheus](http://prometheus.io/)
* [Zabbix](http://www.zabbix.com/)
* [Monit](https://mmonit.com/monit/)
* [Munin](http://munin-monitoring.org/)
* [collectd](https://collectd.org/)
* [Ganglia](http://ganglia.sourceforge.net/)
* [Monitorix](http://www.monitorix.org/)
* [DaemonTools](http://cr.yp.to/daemontools.html)
* [MIG](http://mig.mozilla.org/)
* [Performance Co Pilot](http://pcp.io/)
* [CachetHQ](https://cachethq.io/)
* [StatusPage](https://www.statuspage.io/)
* [cAdvisor](https://github.com/google/cadvisor)
* [Sensu](https://github.com/sensu/sensu)
* [Graphite](http://graphite.wikidot.com/)
* [Grafana](http://grafana.org/)
* [Snap](https://github.com/intelsdi-x/snap)
* [pome](https://github.com/rach/pome)
* [netdata](http://netdata.firehol.org/)


# Presentations

* [Go Present](https://godoc.org/golang.org/x/tools/cmd/present)
* [mdp](https://github.com/visit1985/mdp)
* [reveal.js](http://lab.hakim.se/reveal-js)
* [dzslides](http://paulrouget.com/dzslides/)


# Software Defined Network

* [Calico](http://www.projectcalico.org/)
* [Weave](https://github.com/zettio/weave)
* [Flannel](https://github.com/coreos/flannel)
* [Pipework](https://github.com/jpetazzo/pipework)


# Penetration Tests

* [skipfish](https://code.google.com/p/skipfish/)
* [Burp](https://portswigger.net/burp/)


# Performance/Load tests

* [Gatling](http://gatling-tool.org/)
* [httperf](http://www.hpl.hp.com/research/linux/httperf/)
* [ab](http://httpd.apache.org/docs/2.2/programs/ab.html)
* [Tsung](http://tsung.erlang-projects.org/)
* [slapper](https://github.com/ikruglov/slapper)
* [k6](https://k6.io/)
* [curl-loader](http://curl-loader.sourceforge.net/)
* [wrk](https://github.com/wg/wrk)
* [go-wrk](https://github.com/adjust/go-wrk)
* [boom](https://github.com/rakyll/boom)
* [vegeta](https://github.com/tsenart/vegeta)
* [Bees with machine guns](https://github.com/newsapps/beeswithmachineguns)
* [goad](https://github.com/gophergala2016/goad)
* [hargo](https://github.com/mrichman/hargo)
* [JMeter](http://jmeter.apache.org/)


# Web

## Browser Automation

* [Selenium](http://www.seleniumhq.org/)
* [PhantomJS](http://phantomjs.org/)
* [chromedp](https://github.com/chromedp/chromedp)
* [marionette](https://firefox-source-docs.mozilla.org/testing/marionette/marionette/index.html)


## Secret Management

* [Keywhiz](https://square.github.io/keywhiz/)
* [Vault](https://vaultproject.io/)


# Diagrams

* [Dia](https://wiki.gnome.org/Apps/Dia/)
* [Inkscape](http://inkscape.org/en/)
* [Draw.io](https://www.draw.io/)
* [ditaa](http://ditaa.sourceforge.net/)


## Sequence

* [MscGen](https://en.wikipedia.org/wiki/MscGen)
* [MscLatex](http://satoss.uni.lu/software/mscpackage)
* [Sequence Diagrams](https://www.websequencediagrams.com/)


## Graphs

* [Graphviz](https://graphviz.gitlab.io/)


# HTTP Reverse Proxies / Load balancers

* [vulcand](https://github.com/mailgun/vulcand)
* [happroxy](http://www.haproxy.org/)
* [caddy](https://caddyserver.com/)
* [h2o](https://h2o.examp1e.net/)
* [traefik](https://github.com/EmileVauge/traefik)
* [fabio](https://github.com/eBay/fabio)
* [pound](http://www.apsis.ch/pound)


# HTTP traffic analysis

* [mitm](https://github.com/mitmproxy/mitmproxy)
* [wuzz](https://github.com/asciimoo/wuzz)


# Disk Recovery

* [TestDisk](http://www.cgsecurity.org/wiki/TestDisk)


# Speech Analysis

* [pyttsx](https://github.com/parente/pyttsx)
* [speech_recognition](https://github.com/Uberi/speech_recognition)


# OCR

* [tesseract](https://github.com/tesseract-ocr/tesseract)


# Image Tagging

* [NeuralTalk2](https://github.com/karpathy/neuraltalk2)


# Deep Learning

* [Caffe](http://caffe.berkeleyvision.org/)
* [Keras](http://keras.io/)
* [dnngraph](https://github.com/ajtulloch/dnngraph)
* [Elastic Tought](https://github.com/tleyden/elastic-thought)
* [waifu2x](https://github.com/nagadomi/waifu2x)


# Content Analysis

* [Tika](http://tika.apache.org/)


# PDF

* [PDF Liberation](https://pdfliberation.wordpress.com/)


# Natural Language Processing

* [SyntaxNet](https://github.com/tensorflow/models/tree/master/syntaxnet)


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
