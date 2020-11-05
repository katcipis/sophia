<!-- mdtocstart -->

# Table of Contents

- [Articles](#articles)
    - [Queue](#queue)
        - [Data](#data)
        - [Distributed Systems](#distributed-systems)
        - [Concurrency](#concurrency)
        - [Languages](#languages)
        - [Machine Learning](#machine-learning)
        - [Operational Systems](#operational-systems)
            - [Clive](#clive)
            - [Inferno](#inferno)
        - [Security](#security)
        - [Fuzzy Testing](#fuzzy-testing)
        - [Software Design](#software-design)
        - [Not Classified Yet :-)](#not-classified-yet-)
    - [TODO](#todo)
    - [Done](#done)
        - [2020](#2020)
        - [2019](#2019)
        - [2018](#2018)
        - [2017](#2017)
        - [2016](#2016)

<!-- mdtocend -->

# Articles

## Queue

### Data

* [Architecture of a Database System](https://dsf.berkeley.edu/papers/fntdb07-architecture.pdf)
* [Monarch: Google’s Planet-Scale In-Memory Time Series Database](http://www.vldb.org/pvldb/vol13/p3181-adams.pdf)
* [Spanner, TrueTime and the CAP Theorem](https://ai.google/research/pubs/pub45855)
* [Dremel: Interactive Analysis of Web-Scale Datasets](https://research.google/pubs/pub36632/)
* [Dynamo](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
* [Earlybird: Real-Time Search at Twitter](http://users.umiacs.umd.edu/~jimmylin/publications/Busch_etal_ICDE2012.pdf)
* [The Dataflow Model](http://www.vldb.org/pvldb/vol8/p1792-Akidau.pdf)
* [Interpreting the Data: Parallel Analysis with Sawzall](https://ai.google/research/pubs/pub61)
* [The Case for Learned Index Structures](https://arxiv.org/pdf/1712.01208.pdf)
* [New Algorithms for Heavy Hitters in Data Streams](https://arxiv.org/abs/1603.01733)
* [Memory-Efficient Search Trees for Database Management Systems](http://reports-archive.adm.cs.cmu.edu/anon/2020/CMU-CS-20-101.pdf)
* [Verifiable Data Structures](https://github.com/google/trillian/blob/master/docs/papers/VerifiableDataStructures.pdf)
* [Epidemic algorithms for replicated database maintenance](http://bitsavers.trailing-edge.com/pdf/xerox/parc/techReports/CSL-89-1_Epidemic_Algorithms_for_Replicated_Database_Maintenance.pdf)
* [Adaptive real-time anomaly detection for multi-dimensional streaming data](https://aaltodoc.aalto.fi/bitstream/handle/123456789/25101/master_Saarinen_Inka_2017.pdf)
* [Efficient Summing over Sliding Windows (stream statistics)](http://arxiv.org/pdf/1604.02450v1.pdf)
* [Detecting Change in Data Streams](https://cs.uwaterloo.ca/~shai/vldb04.pdf)
* [Semantics and Evaluation Techniques for Window Aggregates in Data Streams](http://web.cecs.pdx.edu/~tufte/papers/WindowAgg.pdf)
* [Relational Database: A Practical Foundation for Productivity](http://delivery.acm.org/10.1145/1290000/1283937/a1981-codd.pdf)


### Distributed Systems

* [Zero Downtime Release: Disruption-free Load Balancing of a Multi-Billion User Website](https://dl.acm.org/doi/abs/10.1145/3387514.3405885)
* [Paxos made simple](https://www.microsoft.com/en-us/research/publication/paxos-made-simple/)
* [Impossibility of Distributed Consensus with One Faulty Process](http://macs.citadel.edu/rudolphg/csci604/ImpossibilityofConsensus.pdf)
* [Time, Clocks, and the Ordering of Events in a Distributed System](https://www.microsoft.com/en-us/research/uploads/prod/2016/12/Time-Clocks-and-the-Ordering-of-Events-in-a-Distributed-System.pdf)
* [Fundamental Limits of Online Network-Caching](https://arxiv.org/abs/2003.14085)
* [Paxos vs Raft: Have we reached consensus on distributed consensus?](https://arxiv.org/abs/2004.05074)
* [Andromeda: Performance, Isolation, and Velocity at Scale in Cloud Network Virtualization](https://www.usenix.org/system/files/conference/nsdi18/nsdi18-dalton.pdf)
* [Serving DNS using a Peer-to-Peer Lookup Service](https://pdfs.semanticscholar.org/44d8/96d3ffbabb8207de0cfd27de93292068d9cb.pdf)
* [Snap: a Microkernel Approach to Host Networking](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/36f0f9b41e969a00d75da7693571e988996c9f4c.pdf)
* [Google-Wide Profiling: A Continuous Profiling Infrastructure for Data Centers](https://research.google.com/pubs/pub36575.html)
* [Taiji: Managing Global User Traffic for Large-Scale Internet Services at the Edge](https://github.com/copyconstruct/library/blob/master/traffic-engineering/Papers/taiji.pdf)
* [A brief introduction to distributed systems](https://www.cs.vu.nl/~ast/Publications/Papers/computing-2016.pdf)
* [The Intergalactic Computer Network](https://github.com/iandennismiller/intergalactic-computer-network/blob/master/products/intergalactic-computer-network.pdf)
* [A Mathematical Theory of Communication](http://math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf)
* [A Generalised Solution to Distributed Consensus](https://arxiv.org/abs/1902.06776)
* [The DSL for composing functions for FaaS platform](http://ceur-ws.org/Vol-2372/SEIM_2019_paper_20.pdf)
* [End to End arguments in system design](http://web.mit.edu/Saltzer/www/publications/endtoend/endtoend.pdf)
* [A note on distributed systems](https://github.com/papers-we-love/papers-we-love/blob/master/distributed_systems/a-note-on-distributed-computing.pdf)
* [Maintaining the Time in a Distributed System](http://infolab.stanford.edu/pub/cstr/reports/csl/tr/83/247/CSL-TR-83-247.pdf)
* [Life beyond Distributed Transactions: an Apostate’s Opinion](http://adrianmarriott.net/logosroot/papers/LifeBeyondTxns.pdf)
* [Granola: Low-Overhead Distributed Transaction Coordination](https://www.usenix.org/system/files/conference/atc12/atc12-final118.pdf)
* [Minimizing Faulty Executions of Distributed Systems](http://www.eecs.berkeley.edu/~rcs/research/nsdi16.pdf)
* [Tango: Distributed Data Structure on a Log](https://www.microsoft.com/en-us/research/wp-content/uploads/2013/11/Tango.pdf)
* [Paxos Simple](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf)
* [Self-stabilizing Systems in Spite of Distributed Control](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.93.314&rep=rep1&type=pdf)
* [Off-Path TCP Exploits: Global Rate Limit Considered Dangerous](http://www.cs.ucr.edu/~zhiyunq/pub/sec16_TCP_pure_offpath.pdf)
* [Unix Time Sharing](ftp://pdp11.org.ru/pub/unix-archive/Documentation/Papers/BSTJ/bstj57-6-1905.pdf)


### Concurrency

* [Laws for Communicating Parallel Processes](https://dspace.mit.edu/handle/1721.1/41962)
* [The problem with threads](http://www.eecs.berkeley.edu/Pubs/TechRpts/2006/EECS-2006-1.pdf)
* [Concurrent Reading and Writing](https://lamport.azurewebsites.net/pubs/rd-wr.pdf)
* [Static Race Detection and Mutex Safety and Liveness for Go Programs](https://arxiv.org/abs/2004.12859)
* [A Static Verification Framework for Message Passing in Go using Behavioural Types](http://mrg.doc.ic.ac.uk/publications/a-static-verification-framework-for-message-passing-in-go-using-behavioural-types/draft.pdf)
* [Ad Hoc Synchronization Considered Harmful](https://pdfs.semanticscholar.org/5de1/66adfe1ee8a19ce430a9d3435b566bb07185.pdf)
* [SCHEDULING AND LOCKING IN MULTIPROCESSOR REAL-TIME OPERATING SYSTEMS](http://www.cs.unc.edu/~bbb/diss/brandenburg-diss.pdf)
* [Transactional Memory Support for C](http://www.open-std.org/jtc1/sc22/wg14/www/docs/n1961.pdf)
* [Kill-Safe Synchronization Abstractions](http://www.cs.utah.edu/plt/publications/pldi04-ff.pdf)
* [The Theory and Practice of Concurrency](http://www.cs.ox.ac.uk/bill.roscoe/publications/68b.pdf)
* [Why events are a bad idea](http://www.cs.berkeley.edu/~brewer/papers/threads-hotos-2003.pdf)


### Languages

* [An Incremental Approach to Compiler Construction](http://scheme2006.cs.uchicago.edu/11-ghuloum.pdf)
* [The Next 700 Programming Languages](https://www.cs.cmu.edu/~crary/819-f09/Landin66.pdf)
* [Growing a Language](https://www.cs.virginia.edu/~evans/cs655/readings/steele.pdf)
* [Fundamental Concepts in Programming Languages](https://www.itu.dk/courses/BPRD/E2009/fundamental-1967.pdf)
* [Can Programming Be Liberated from the von Neumann Style?](http://wwwusers.di.uniroma1.it/~lpara/LETTURE/backus.pdf)
* [Why Functional Programming Matters](http://worrydream.com/refs/Hughes-WhyFunctionalProgrammingMatters.pdf)
* [Meta II A Syntax Oriented Compiler Writing Language](http://www.ibm-1401.info/Meta-II-schorre.pdf)
* [On Certain Properties of Grammars](http://www.diku.dk/hjemmesider/ansatte/henglein/papers/chomsky1959.pdf)
* [Scheme with Classes, Mixins, and Traits](https://www.cs.utah.edu/plt/publications/aplas06-fff.pdf)
* [The Next 700 Programming Languages](https://www.cs.cmu.edu/~crary/819-f09/Landin66.pdf)
* [On-the-fly garbage collection: an exercise in cooperation](http://research.microsoft.com/en-us/um/people/lamport/pubs/garbage.pdf)
* [Fundamental Concepts in Programming Languages](http://www.itu.dk/courses/BPRD/E2009/fundamental-1967.pdf)
* [Languages as Libraries](http://www.cs.utah.edu/plt/publications/pldi11-tscff.pdf)
* [Advanced Macrology and the Implementation of Typed Scheme](http://www.ccs.neu.edu/racket/pubs/scheme2007-ctf.pdf)
* [The Galois Connection between Syntax and Semantics](http://www.logicmatters.net/resources/pdfs/Galois.pdf)
* [A Comparative Study of Language Support for Generic Programming](http://www.osl.iu.edu/publications/prints/2003/comparing_generic_programming03.pdf)
* [Advanced Procedural Modeling of Architecture](http://research.michael-schwarz.com/publ/files/cgapp-sig15.pdf)


### Machine Learning

* [Hidden Technical Debt in Machine Learning Systems](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf)
* [150 Successful Machine Learning Models: 6 Lessons Learned at Booking.com](http://delivery.acm.org/10.1145/3340000/3330744/p1743-bernardi.pdf)
* [Understanding deep learning requires rethinking generalization](https://arxiv.org/abs/1611.03530)
* [A Closer Look at Memorization in Deep Networks](https://arxiv.org/pdf/1706.05394.pdf)
* [Deep Learning Scaling is Predictable, Empirically](https://arxiv.org/abs/1712.00409)
* [Universal Language Model Fine-tuning for Text Classification](https://arxiv.org/abs/1801.06146)


### Operational Systems

* [The Linux Scheduler: a Decade of Wasted Cores](http://www.ece.ubc.ca/~sasha/papers/eurosys16-final29.pdf)
* [UNIX Implementation](https://users.soe.ucsc.edu/~sbrandt/221/Papers/History/thompson-bstj78.pdf)


#### Clive

* [The Clive Operating System](http://lsub.org/export/clivesys.pdf)
* [Channels done right](http://lsub.org/export/belt.pdf)


#### Inferno

* [Styx on a brick](http://www.vitanuova.com/inferno/papers/lego.pdf)
* [Limbo profilers in Inferno](http://www.vitanuova.com/inferno/papers/lprof.pdf)
* [The Several Inferno Ports](http://www.vitanuova.com/inferno/papers/port.html)



### Security


* [Provably Secure Password-Authenticated Key Exchange Using Diffie-Hellman](https://www.iacr.org/archive/eurocrypt2000/1807/18070157-new.pdf)
* [SDSI - A Simple Distributed Security Infrastructure](https://people.csail.mit.edu/rivest/sdsi10.html)
* [HTTP Desync Attacks: Request Smuggling Reborn](https://portswigger.net/kb/papers/z7ow0oy8/http-desync-attacks.pdf)
* [CacheOut: Leaking Data on Intel CPUs via Cache Evictions](https://cacheoutattack.com/CacheOut.pdf)
* [Strong Password-Only Authenticated Key Exchange](http://www.jablon.org/jab96.pdf)


### Fuzzy Testing

* [Ankou: Guiding Grey-box Fuzzing towards Combinatorial Difference](https://www.jiliac.com/files/ankou-icse2020.pdf)


### Software Design

* [Notes on Structured Programming](https://www.cs.utexas.edu/~EWD/ewd02xx/EWD249.PDF)


### Not Classified Yet :-)

* [The Computer as a Communication Device](https://internetat50.com/references/Licklider_Taylor_The-Computer-As-A-Communications-Device.pdf)
* [Man Computer Symbiosis](https://internetat50.com/references/Licklider_Man-Computer-Symbiosis.pdf)
* [John Carmack Interviews](http://fd.fabiensanglard.net/doom3/pdfs/johnc-interviews.pdf)
* [Winners Curse](https://openreview.net/pdf?id=rJWF0Fywf)
* [Why Software Projects need Heroes](https://arxiv.org/abs/1904.09954)
* [Functional Data Structures](https://www.cs.cmu.edu/~rwh/theses/okasaki.pdf)
* [Specifying and Model Checking Workflows of Single Page Applications with TLA+](https://arxiv.org/abs/2005.05627)
* [Combining reverse debugging and live programming towards visual thinking in computer programming](https://scholar.sun.ac.za/bitstream/handle/10019.1/96853/coetzee_combining_2015.pdf)
* [Augmenting Human Intellect](http://www.dougengelbart.org/pubs/papers/scanned/Doug_Engelbart-AugmentingHumanIntellect.pdf)
* [STEPS Toward the Reinvention of Programming](http://www.vpri.org/pdf/tr2012001_steps.pdf)
* [What Every Programmer Should Know About Memory](http://www.akkadia.org/drepper/cpumemory.pdf)
* [What Every Computer Scientist Should Know About Floating-Point Arithmetic](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html)
* [Why and How Java Developers Break APIs](https://arxiv.org/pdf/1801.05198.pdf)
* [Industrial Society and Its Future](http://editions-hache.com/essais/pdf/kaczynski2.pdf)
* [Toward Simplifying Application Development, in a Dozen Lessons](http://melconway.com/Home/pdf/simplify.pdf)
* [Prog in the small and in the large](http://www.cs.umd.edu/class/spring2005/cmsc838p/General/pitl.pdf)
* [Google’s Hybrid Approach to Research](http://static.googleusercontent.com/media/research.google.com/en/us/pubs/archive/38149.pdf)
* [How Did Software Get So Reliable Without Proof?](http://www.gwern.net/docs/1996-hoare.pdf)


## TODO

* [Hints and Principles for Computer System Design](https://www.microsoft.com/en-us/research/publication/hints-and-principles-for-computer-system-design-3/)
* [Designing Cluster Schedulers for Internet-Scale Services](https://queue.acm.org/detail.cfm?id=3199609)
* [Simple Testing Can Prevent Most Critical Failures](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf)
* [On Systems Design](https://scholar.harvard.edu/files/waldo/files/ps-2006-6.pdf)
* [A Plea for Lean Software](https://cr.yp.to/bib/1995/wirth.pdf)
* [The Emperor's Old Clothes](https://dl.acm.org/doi/10.1145/358549.358561)
* [Our Human Condition "From Space"](http://www.vpri.org/pdf/m2003001_human_cond.pdf)


## Done

### 2020

* [How](https://internetat50.com/references/Kay_How.pdf)
* [The Text Editor sam](https://plan9.io/sys/doc/sam/sam.html)
* [The Byzantine Generals Problem](http://lamport.azurewebsites.net/pubs/byz.pdf)
* [Google Workloads for Consumer Devices](https://people.inf.ethz.ch/omutlu/pub/Google-consumer-workloads-data-movement-and-PIM_asplos18.pdf)
* [WireGuard: Next Generation Kernel Network Tunnel](https://www.wireguard.com/papers/wireguard.pdf)
* [CPU bandwidth control for CFS](https://research.google/pubs/pub36669/)
* [Write your Own Virtual Machine](https://justinmeiners.github.io/lc3-vm/)
* [Revisiting Coroutines](http://www.inf.puc-rio.br/~roberto/docs/MCC15-04.pdf)
* [The Computer Scientist as Toolsmith](https://cs.unc.edu/xcms/wpfiles/toolsmith/The_Computer_Scientist_as_Toolsmith.pdf)
* [An Inside Look at Google BigQuery](https://cloud.google.com/files/BigQueryTechnicalWP.pdf)
* [The Computer Scientist as Toolsmith 2](http://www.cs.unc.edu/~brooks/Toolsmith-CACM.pdf)


### 2019

* [Dapper, a Large-Scale Distributed Systems Tracing Infrastructure](https://research.google.com/pubs/pub36356.html)
* [Deconstructing process isolation](https://www.researchgate.net/publication/220773465_Deconstructing_process_isolation)
* [Spectre is here to stay: An analysis of side-channels and speculative execution](https://arxiv.org/abs/1902.05178)
* [Understanding Real-World Concurrency Bugs in Go](https://songlh.github.io/paper/go-study.pdf)


### 2018

* [Spanner](https://research.google.com/archive/spanner-osdi2012.pdf)
* [Programming Considered as a Human Activity](https://www.cs.utexas.edu/~EWD/transcriptions/EWD01xx/EWD117.html)
* [The Humble Programmer](https://www.cs.utexas.edu/~EWD/transcriptions/EWD03xx/EWD340.html)
* [Program Development Under Inferno](http://www.vitanuova.com/inferno/papers/dev.html)
* [The Inferno Shell](http://www.vitanuova.com/inferno/papers/sh.html)
* [Dis Virtual Machine specs](http://www.vitanuova.com/inferno/papers/dis.html)
* [NDBench: Benchmarking Microservices at Scale](https://arxiv.org/pdf/1807.10792.pdf)
* [The design of the Inferno virtual machine](http://doc.cat-v.org/inferno/4th_edition/dis_VM_design)
* [Unix and Beyond](http://cse.unl.edu/~witty/class/csce351/howto/ken_thompson.pdf)
* [The Inferno Operating System](http://doc.cat-v.org/inferno/4th_edition/inferno_OS)
* [The Future of Computing: Logic or Biology](https://lamport.azurewebsites.net/pubs/future-of-computing.pdf)
* [Rc - The Plan 9 Shell](http://doc.cat-v.org/plan_9/4th_edition/papers/rc)
* [CockroachDB](http://cs.ulb.ac.be/public/_media/teaching/cockroachdb_2017.pdf)
* [Intel® Clear Containers: A breakthrough combination of speed and workload isolation](http://www.cse.iitb.ac.in/synerg/lib/exe/fetch.php?media=public:students:chandra:vmscontainers_wp_final.pdf)
* [Mk: a successor to make](http://doc.cat-v.org/bell_labs/mk/mk.pdf)
* [A General Purpose File System for Secondary Storage](http://multicians.org/fjcc4.html)
* [Tea, A Tiny Encription Algorithm](https://www.movable-type.co.uk/scripts/tea.pdf)
* [ACME](http://www.vitanuova.com/inferno/papers/acme.pdf)
* [Large-scale cluster management at Google with Borg](https://research.google.com/pubs/pub43438.html)
* [Recursive Functions of Symbolic Expressions and Their Computation by Machine](http://www-formal.stanford.edu/jmc/recursive.pdf)
* [Venti: a new approach to archival storage](http://doc.cat-v.org/plan_9/4th_edition/papers/venti/)
* [The Limbo Programming Language](http://www.vitanuova.com/inferno/papers/limbo.html)

### 2017

* [Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
* [What is Software Design](http://user.it.uu.se/~carle/softcraft/notes/Reeve_SourceCodeIsTheDesign.pdf)
* [Breaking Audio Captchas](http://www.captcha.net/Breaking_Audio_CAPTCHAs.pdf)
* [UnCaptcha](https://www.usenix.org/system/files/conference/woot17/woot17-paper-bock.pdf)
* [Plan9 from Bell Labs](http://doc.cat-v.org/plan_9/4th_edition/papers/9)
* [Security in plan 9](http://doc.cat-v.org/plan_9/4th_edition/papers/auth)
* [The Styx Architecture for Distributed Systems](http://doc.cat-v.org/inferno/4th_edition/styx)
* [The Organization of Networks in Plan 9](http://doc.cat-v.org/plan_9/4th_edition/papers/net/)
* [The use of namespaces in Plan 9](http://doc.cat-v.org/plan_9/4th_edition/papers/names)
* [Lexical Filenames in Plan 9](http://doc.cat-v.org/plan_9/4th_edition/papers/lexnames)
* [The humble programmer](http://www.cs.utexas.edu/users/EWD/transcriptions/EWD03xx/EWD340.html)
* [A Relational Model of Data for Large Shared Data Banks](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf)
* [WiscKey: Separating Keys from Values in SSD-conscious Storage](https://www.usenix.org/system/files/conference/fast16/fast16-papers-lu.pdf)
* [The Power Of Context](http://www.vpri.org/pdf/m2004001_power.pdf)
* [The USE method](http://www.brendangregg.com/usemethod.html)

### 2016

* [Building LinkedIn’s Real-time Activity Data Pipeline](http://sites.computer.org/debull/A12june/pipeline.pdf)
* [On Designing and Deploying Internet-Scale Services](https://www.usenix.org/legacy/event/lisa07/tech/full_papers/hamilton/hamilton_html/)
* [Process file system and process in Unix V](https://www.usenix.org/sites/default/files/usenix_winter91_faulkner.pdf)
* [Using a hierarchy of Domain Specific Languages in complex software systems design](http://arxiv.org/pdf/cs/0409016.pdf)
* [CSP Hoare](http://spinroot.com/courses/summer/Papers/hoare_1978.pdf)
* [Go: A look behind the scenes](http://doc.cat-v.org/programming/go/papers/Paper__aigner_baumgartner.pdf)
* [I’m not a human: Breaking the Google reCAPTCHA](https://www.blackhat.com/docs/asia-16/materials/asia-16-Sivakorn-Im-Not-a-Human-Breaking-the-Google-reCAPTCHA-wp.pdf)
* [A hierarchical approach to wrapper induction](http://www.isi.edu/integration/papers/muslea99-agents.pdf)
* [Extracting Web Data Using Instance-Based Learning](https://www.cs.uic.edu/~yzhai/paper/WISE05-instance-extract.pdf)
* [On the Criteria To Be Used in Decomposing Systems into Modules](https://web.archive.org/web/20120223013018/https://www.cs.umd.edu/class/spring2003/cmsc838p/Design/criteria.pdf)
