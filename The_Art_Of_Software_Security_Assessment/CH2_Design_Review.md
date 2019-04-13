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
The Shatter class of vulneabilities stems from a failure to decompose a design along trust boundaries (more technical details in Chapter 12). Each Windows desktop has a single messaging queue to handle all GUI-related activities. The Windows SetTimer() function can be used to send a WM_TIMER message, which will invoke an abirary function pointer upon receipt by the default message handler. Using another Windows API that manipulates the content of window elements, it is possible to insert data in the address space of another process. Combining the delievry of arbitrary data and the call of a function, an attackr can execute arbitrary code in any process on the same desktop. This results in a privilege escalaton vulnerability.

The messaging design was accurate, understandable, and strongly cohesive for a single-user OS. However, when used for a multi-user OS, the communication across a desktop must be considered an attack vector. In that context, the messaging queue strongly couples different trust domains, which results in vulnerabilities that exploit the desktop as a public interface. This vulnerability class illustrates the difficulty in adding security to an existing design.

### Exploiting Transitive Trust


### Failure Handling

# Enforcing Security Policy
## Authentication 
## Authorization
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
