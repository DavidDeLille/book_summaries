# Chapter 2 - DESIGN REVIEW
## Introduction
Developers like design reviews because they map closely to formal software development methodologies. Code auditors think it just gets in the way of the real work. The process has value for both.  
Design review is a tool for identifying architecture vulnerabilities and prioritising components for implementation review. It is part of the complete review process.
## Software Design Fundamentals
### Algorithms
Software engineeering can be thought of as the process of developing and implementing algorithms. It focuses on developing key program algorithms and data structures, and specifying problem domain logic.
#### Problem Domain Logic / Business Logic
Provides rules that a program follows as it processes data.
#### Key Algorithms
Performance requirements can dictate the choice of algorithms and data structures. These choices should be evaluated to predict potential vulnerabilities.
### Abstraction and Decomposition
Abstraction = method for reducing the complexity of a system to make it more manageable.

Isolate the most important elements and remove unnecessary details. This enables you to establish hierachies of related systems, concepts, and processes.

Decomposition (a.k.a. factoring) = process of defining the generalisations and classifications that compose an abstraction.

* Top-down (a.k.a. specialisation): breaking up a larger system into smaller, more manageable parts.
* Bottom-up (a.k.a. generalisation): identifying similarities in a number of components and developing a high-level abstraction that applies to all of them.

Standard top-down decomposition progression: application, module, class, function.
### Trust Relationships
Trust relationship = trust associated with a communication between multiple parties.

Every communication has a trust relationship. Complete trust means complete access to the exposed functionality. For security purpposes, we are concerned with situations where trust needs to be resricted.

Trust boundary = limitations imposed on each party in a communication that defines the limited subset of functionality that can be accessed.

Trust domain = region of shared trust, distinguished by trust boundaries.

Trust model = abstraction that accounts for a system's trust domains, boundaries, and relationships. It is a component of the application's security policy.

Trust boundaries tend to be module boundaries.

Privileges = degrees of trust (as opposed to absolute trust)

#### Simple Trust Boundaries
FIgure 2-1 shows the simple trust boundaries in a multi-user OS. The accompanying text describes this trust model.

Every system needs an absolutely **trusted authority** (e.g. root/Administrator account), because someone needs to be responsile for the system.

#### Complex Trust Boundaries
Defining and applying a trust model has a huge impact on a software design. The feasibility study and requirements-gathering phases must adequately identify and define users' security expectations and the associated factors of the target environment. The trust model must be robust enough to meet these reqs, but not too complex to be implemented/applied.

#### Chain of Trust
Chain-of-trust relationship = transitive trust relationship (A trusts B, and B trusts C ==> A trusts C)

SSL is an example of chain of trust. Any compromise of a CA can be used to impersonate any website.

Sometimes chain of trust is the only option, but an auditor needs to review the impact of this model and whether it is appropriate. It often results in complex and subtle trust relationships that attackers an exploit.

#### Defence in Depth

### Principles of Software Design
### Fundamental Design Flaws 
## Enforcing Security Policy
### Authentication 
### Authorization
### Accountability
### Confidentiality
### Integrity
### Availability
## Threat Modeling
### Information Collection
### Application Architecture Modeling
### Threat Identification 
### Documentation of Findings
### Prioritizing the Implementation Review
## Summary 
