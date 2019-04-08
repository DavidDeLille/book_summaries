# Vulnerabilities
= Flaw that allows an attacker to do something malicious.  
Vulnerabilities are (approxiate) subset of software bugs.  
Eploiting = abusing a vulnerability.  
Exploit = eternal program or script that abuses a vulnerability.  
## Security Policies
= List of what's allowed and what's forbidden.  
Can be formal (specification/constraints or a policy document) or informal (loose expectations).  
## Security Expectations
### Confidentiality
Requires information to be kept private.  
System contains information about people => privacy concerns.  
Often requires authentication, authorisation, and encryption.  
### Integrity
= Trustworthiness and correctness of data.  
Involves both the content and the source of the data.  
Often requires authentication and authorisation.  
Confidentiality is about reading sensitive data, integrity is about writing sensitive data.  
### Availability
= Capability to use information and resources.  
# The Necessity of Auditing
Multiple ways to test security: automated code analysis, security unit testing, and manual code audits.  
Auditing = process of analysing application code (source/binary) to uncover vulnerabilities.  
Table 1-1 lists situations in which auditing makes sense.  
## Auditing Versus Black Box Testing
Black box testing = method of evaluating a software system by manipulating only its exposed interfaces.  
Typically involves generating specially crafted inputs that result in unexpected behaviour.  
Automated black box testing is called fuzzing.  
Advantages: quick and potential immediate results.  
Disadvantages: no idea of code coverage.  
Code auditing + black box testing provides maximum result in minimum time.  
## Code Auditing and the Development Life Cycle
Auditing can occur at any point in the System Development Life Cycle (SDLC), but the cost of finding and fixing vulnerabilities can vary.  
6 phases of SDLC.  
Waterfall develpment model follows these phases strictly.  
Agile development model goes through multiple iterations.  
# Classifying Vulnerabilities
Vulnerability class = set of vulnerabilities that shares some unifying comminality (a pattern or concept tat isolates a specific feature shared by several different software flaws).  
Mental devices for conceptualising softare flaws.  
There is no perfect, non-overlapping taxonomy.  
## Design Vulnerabilities
= Problem that arises from a fundamental mistake/oversight in the software's design.  
Software was designed to do the wrong thing.  
SDLC phases 1, 2, and 3.  
A.k.a. high-level vulnerabilities, architectural flaws, problems with program requirements/constraints.  
Requirements = list of objectives a software system must meet to accomplish the goals of the creators.  
Specifications = plans for how the program should be constructed to meet the requirements.  
## Implementation Vulnerabilities
The code is doing what it should, but there is a security problem in the way the operation is carried out.  
SDLC phases 4 and 5.  
A.k.a. low-level flaws, technical flaws.
Identifying technical flaws is one fo the primary charges of the code review process.  
## Operational Vulnerabilities
= Security problems that arise through the operational procedures and general use of a piece of software in a specific environment.  
These vulnerabilities are not present in the source code, because they are rooted in how the software interacts with its environment.  
SDLC phase 6 (with some overlap into phase 5)
## Gray Areas
Distinctions between these classes are not always clear.  
* Design vulnerabilities = high-level issues with program architecture, requirements, base interfaces, and key algorithms.  
* Implementation vulnerabilities = issues in the design of low-level program pieces (such as parts of individual functions or classes) and more complex logical elements not normally addresses in the design specification (logic vulnerabilities).  
* Operation vulnerabilities = issues that deal with unsafe deployment and configuration of software, unsound managemnt and administration practices surrounding software, issues with supporting compoonents such as application and web servers, and direct attacks on the software's users.

Plenty of room for interpretation and overlap between these definitions.  
# Common Threads
These common threads underlying vulnerabilities can reveal where and why vulnerabilities are most likely to surface.
## Input and Data Flow
Most vulnerabilities result from malicious data triggering unexpected behaviour. This data can come from many sources, pass through many interfaces, pass through multiple commponents, and be modified before triggering the vulnerability. As a result, considering the flow of data throughout the system components is one of the most useful attributes in reviewing a software system.  
The process of tracing data flow is central to design and implementation reviews. It is the main way to evaluate the serious threat of user-malleable data:
1. Identify where user-malleable data enters the system through an interface.  
2. Study the different ways in which user-malleable data can travel trhrough the system.  
3. Look for any potentially exploitable code that acts on the data.

A vulnerability might be unexploitable due to prior input sanitisation.  
Tracing data flow in a real-world application can be difficult, because coplex systems often evolved organically, resulting in highly fragmented data flows.  
## Trust Relationships
Integral to the flow of data. The level of trust between components determines the amount of validation of the data.  
A trusted interface/component is believed to be impervious to malicious interference and to conform to assumptions about its behaviour and data.  
Misplaced trust can lead to a cascade of security issues.  
Trust is transitive.  
## Assumptions and Misplaced Trust
Considering unfounded assumptions made by programmers can be useful when looking at vulnerabilities.  
It is closely related to misplaced trust.  
### Input
Software often places too much trust in other components and makes assumptions about data and its source.  
Attackers will consider every possible input that can be entered.  
An unanticipated change in the software's runtime properties can often be leveraged to gain more influence on the program.  
## Interfaces
= Mechanisms by which components communicate with each other and the outside world.  
Many vulnerabilities result from assumptions that only trusted peers can interfaces or how difficulty reaching an interface is.  
Reasons why trust in interfaces can be misplaced:
* The method of exposing the interface doesn't provide enough protection from attackers
* The method of exposing the interface is misused or misconfigured (even if it is reliable; e.g. service of the OS). A vulnerability in the base system may also be exploitable.
* The interface is wrongly assumed to be too difficult to access by an attacker.
## Environmental Attacks
Software systems run as one or more programs supported by a larger computing environment, including OSes, hardware architectures, networks, file systems, databases, and users.
Wrong assumptions or misunderstandings about the potential security issues of each technology in the underlying environment can cause vulnerabilities.  
## Exceptional Conditions
An exceptional condition occurs when an attacker can cause an unexpected change in the program's normal control flow via external means.
# Summary
Goals of the chapter: 
* Have a good understanding of what can go wrong in software.
* Understand the terminology.
* Understand the need for auditing.
* Become familiar with the different aspects of the process.
