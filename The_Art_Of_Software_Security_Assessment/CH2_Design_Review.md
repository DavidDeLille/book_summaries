# Introduction
Developers like design reviews because they map closely to formal software development methodologies. Code auditors think it just gets in the way of the real work. The process has value for both.  
Design review is a tool for identifying architecture vulnerabilities and prioritising components for implementation review. It is part of the complete review process.
# Software Design Fundamentals
## Algorithms
Software engineeering can be thought of as the process of developing and implementing algorithms. It focuses on developing key program algorithms and data structures, and specifying problem domain logic.
### Problem Domain Logic / Business Logic
Provides rules that a program follows as it processes data.
### Key Algorithms
Performance requirements can dictate the choice of algorithms and data structures. These choices should be evaluated to predict potential vulnerabilities.
## Abstraction and Decomposition
Abstraction = method for reducing the complexity of a system to make it more manageable.

Isolate the most important elements and remove unnecessary details. This enables you to establish hierachies of related systems, concepts, and processes.

Decomposition (a.k.a. factoring) = process of defining the generalisations and classifications that compose an abstraction.  
* Top-down (a.k.a. specialisation): breaking up a larger system into smaller, more manageable parts.
* Bottom-up (a.k.a. generalisation): identifying similarities in a number of components and developing a high-level abstraction that applies to all of them.  
Standard top-down decomposition progression: application, module, class, function.

## Trust Relationships
Trust relationship = trust associated with a communication between multiple parties.  
Every communication has a trust relationship. Complete trust means complete access to the exposed functionality. For security purpposes, we are concerned with situations where trust needs to be resricted.

Trust boundary = limitations imposed on each party in a communication that defines the limited subset of functionality that can be accessed.  
Trust domain = region of shared trust, distinguished by trust boundaries.  
Trust model = abstraction that accounts for a system's trust domains, boundaries, and relationships. It is a component of the application's security policy.

Trust boundaries tend to be module boundaries.

Privileges = degrees of trust (as opposed to absolute trust)

### Simple Trust Boundaries
FIgure 2-1 shows the simple trust boundaries in a multi-user OS. The accompanying text describes this trust model.

Every system needs an absolutely **trusted authority** (e.g. root/Administrator account), because someone needs to be responsile for the system.

### Complex Trust Boundaries
Defining and applying a trust model has a huge impact on a software design. The feasibility study and requirements-gathering phases must adequately identify and define users' security expectations and the associated factors of the target environment. The trust model must be robust enough to meet these reqs, but not too complex to be implemented/applied.

### Chain of Trust
Chain-of-trust relationship = transitive trust relationship (A trusts B, and B trusts C ==> A trusts C)

SSL is an example of chain of trust. Any compromise of a CA can be used to impersonate any website.

Sometimes chain of trust is the only option, but an auditor needs to review the impact of this model and whether it is appropriate. It often results in complex and subtle trust relationships that attackers an exploit.

### Defence in Depth
Defence in depth = layering protections so that the compromise of any component is mitigated by other controls.

Layered defences can inform component piorities during the review.

## Principles of Software Design
 Thre are many software development methodologies, but they share certain commonly accepted prinicples.
 
### Accuracy
= How effectively design abstraction meet the associated requirements.  
This includes both how closely the abstractions model the requirements and how easily they can be implemented.

Differences between a software design and the implementation indicate weaknesses in the design abstractions. When developers need to make assumptions outside the intended design, not communicating these assumptions can cause vulnerabilities. Look for inadequately defined design areas, or areas that place unreasonable expectations on programmers.

### Clarity
= How well the design decomposes the problem and how clean/self-evident the abstractions are.  
This includes the quality of the documentation.

Poor understanding of the design can lead to vulnerabilities similar to those of an inaccurate design. Look for complex or poorly documented design components (see also variable relatiknships in Chapter 7).

### Loose Coupling
Coupling = the level of communication between modules and exposure of their internal interface.  
Loosely coupled modules have well-defined public interfaces.  
Strongly coupled modules have complex interdependencies and expose important internal interfaces.

Strongly coupled modules place a high degree of trust in each other and rarely perform data validation on their communication. Look for strong intermodule coupling across trust boundaries.

### Strong Cohesion
Cohesion = a module's internal consistency; primarily the degree to which a module's interfaces handle a related set of activities.  
A strongly cohesive module only handles closely realted activities.

When a design fails to decompose modules along trust boundaries, vulnerabilites similar to those related to coupling occur, only within the same module. This happens when security is tacked onto a design at the end. Look for modules that have to operate in multiple trust domains.

## Fundamental Design Flaws
This section contains examples of these concepts affecting security. The categorisation is subjective, so focus on the security impact instead.

### Exploiting Strong Coupling
The Shatter class of vulneabilities stems from a failure to decompose a design along trust boundaries (more technical details in Chapter 12). Each Windows desktop has a single messaging queue to handle all GUI-related activities. The Windows SetTimer() function can be used to send a WM_TIMER message, which will invoke an abirary function pointer upon receipt by the default message handler. Using another Windows API that manipulates the content of window elements (e.g. Clipboard), it is possible to insert data in the address space of another process. Combining the delivery of arbitrary data and the call of a function, an attacker can execute arbitrary code in any process on the same desktop. This results in a privilege escalaton vulnerability.

The messaging design was accurate, understandable, and strongly cohesive for a single-user OS. However, when used for a multi-user OS, the communication across a desktop must be considered an attack vector. In that context, the messaging queue strongly couples different trust domains, which results in vulnerabilities that exploit the desktop as a public interface. This vulnerability class illustrates the difficulty in adding security to an existing design.

### Exploiting Transitive Trust
Automountd is an RPC program on certain version of Solarist hat runs as root. It allows root to specify a command as part of a mounting operation. It only listens on three protected loopback transports and only accepts commands from root.

Rpc.statd also runs as root and listens on TCP and UDP interfaces. It is meant to montior NFS servers and send out a notification if they go down.When it receives such a notification, it will contact a host and call an RPC program number.

An attacker can tell rpc.statd that an NFS server has crashed and have it contact automountd on the local machine with an RPC message. With some manipulation, this message can be decoded as a valid automountd request. Since it is coming from root on the loopback interface, automountd trusts this message and executes the command. This attack exploits the implicit trust between all processes running under the same account. Automountd also is lenient in the messages it receives due to the perceived trust of the interface.

### Failure Handling
Proper failure handling is essential for a clear and accurate usability in a software design. Users expect that the application handles irregular conditions properly and provides information to resolve the issue. However, this level of usaility might be in oppsition to the security requirements.

Usability dictates that a network application should attempt to recover from a fault and continue processing. If this isn't possible, detailed information about the error should be displayed.

For a security standpoint, detection of a fault should terminate the client session with minimum feedback and log the details extensively. This approach assumes the fault is caused by an attacker. Working around the fault and continue processing would play into the attacker's hands. In this case, the security requirements supersede the usability requirements.

# Enforcing Security Policy
Chapter 1 discusses security expectations. A security policy is impemented by identifying and enforcing trust boundaries. Enforcement is broken up into six main types:

## Authentication 
= Process of determining identity.  
Not just for humans; also servers, components, etc.

### Common Vulnerabilities
* Lack of authentication: Not always obvious; perhaps an attacker can access a presumably internal interface. Best practice is to centralise the authentication in the design.
* Untrustworthy credentials: E.g. client-side authentication. 
  * The UNIX RPC daemon rexd (remote execution daemon) allows remote users to run programs as user they specify (not root); running /bin/sh as as user bin would lead to root.
  * AUTH_UNIX authentication ask the client to provide a record of the user and their group ID. The defaukt installation of sadmind on Solaris allowed this scheme (www.securityfocus.com/bid/2354/info).
  * Many daemons use source IP address to establish identity. UDP can be trivially spoofed. TCP can also be spoofed, intercepted, or hijacked. E.g. rshd, rlogind, and sshd can be configured to trust source IP.
* Insufficient validation: 
  * Not often in username/password authentication. More likely a design flaw in programmatic authentication bvetween systems. E.g. onyl authentication in one direction.
  * Homemade authentication with cryptographic primitives is hard to get right. See example of FWN/1 protocol of Firewall-1.

## Authorization
= Process of determining permissions based on identity.


## Accountability
## Confidentiality
## Integrity
## Availability
# Threat Modeling
## Information Collection
## Application Architecture Modeling
## Threat Identification 
## Documentation of Findings
## Prioritizing the Implementation Review
# Summary 
