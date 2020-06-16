# Lecture 10 Security and Clouds
## Importance of Security
* If systems <font color=pink>(Grids/Clouds/outsourced infrastructure)</font> are not secure:
    * Large communities will not engage
        * Medical community, industry, financial community
    * Expensive to repeat some experiments
        * Huge machines running large simulations for several years
    * Legal and ethical issues possible to be violated with all sorts of consequences
        * <font color=pink>E.g. Data protection act violations and fines incurred.</font>
    * Trust is easily lost and hard to re-establish 
## Challenge of Security
* Grids and Clouds(IaaS) allow users to compile codes that do stuff on physical/virtual machines
    * Highly secure supercomputing facilities compromised by single user PCs/laptops
    * Need security technologies that scales to meet wide variety of application
* Should try to develop generic security solutions
    * Avoid all application areas re-inventing their own solutions
* Clouds allow scenarios that stretch inter-organizational security
    * Policies that restrict access to and usage of resources based on pre-identified users, resources
        * <font color=pink>Groups/tenancy</font>
    * What if new resources added, new users added, old users go?
        * <font color=pink>Over-subscription issues</font>
        * <font color=pink>User management</font>
    * What is organizations decide to change policies governing access to and usage of resources, or bring their data back inside of their firewall?
    * What if you share a tenancy with a noisy neighbor?
    * The multi-faceted challenges of 'Life beyond the organizational firewall'
## Prelude to security
* Meaning of security
    * Secure from whom?
    * Secure against what?
    * Secure for how long?
    * Note that security technology not means secure system
## Technical Challenges of Security
* Several key terms that associated with security
    * Authentication
    * Authorization
    * Audit/accounting
    * Confidentiality 
    * Privacy
    * Fabric management
    * Trust 
### <font color=red>Authentication</font>
* <font color=red>Authentication</font> is the establishment and propagation of a user's identity in the system
    * <font color=pink>E.g. Site X can check that user Y is attempting to gain access to it's resources</font>
    * Local username/password
    * Centralized vs decentralized systems
    * <font color=red>Public Key Infrastructures(PKI)</font> underpins many systems. <font color=pink>Based on public key cryptography</font>
    #### <font color=red>Public Key Cryptography</font>
    * Also called <font color=red>Asymmetric Cryptography</font>
        * Two distinct keys
            * One that must be kept private
            * One that can be made public
        * Two keys complementary, but essnetial that cannot find out value of private key from public key
            * With private keys can digitally sign messages, documents. And validate them with associated public keys
    * Public Key Cryptography simplifies key management
        * Do not need to have many keys for long time
            * The longer keys are left in storage, more likelihood of their being compromised.
            * Only Private Key needs to be kept long term and kept securely.
    #### <font color=red>Public Key Certificates</font>
    * Mechanism connecting public key to user with corresponding private key is <font color=red>Public Key Certificate</font>
        * Public key certificate contains public key and identifies the user with the corresponding private key
    #### <font color=red>*Certification Authority*</font>
    * Central component of PKI is <font color=red>*Certification Authority*(CA)</font>
        * CA has numerous responsibilities
            * Policy and procedures
            * Issuing certificates
            * Revoking certificates
            * Storing and archiving
    >Example of a simple CA
    >><img src='Simple_CA.png' width='50%'>
### <font color=red>Authorization</font>
* <font color=red>Authorization</font> is concerned with controlling access to resources based on policy
    * Can this user invoke this service, make use of this data?
    * Complementary to authentication
        * Know it is this user, now we can restrict/enforce what they can/cannot do
* Many different approaches for authorization
    * Group Based Access Control
    * Role Based Access Control(RBAC)
    * Identity Based Access Control(IBAC)
    * Attribute Based Access Control(ABAC)
 * Authorization and Cloulds
    * Authorization typically applies to services/data deployed on Clouds when they are running.
* How to do authorization
    * Defining what they can do and define and enforce rules
        * Each site will have different rules/regulations
    * Often realized through <font color=red>Virtual Organization(VO)</font>
        * Collection of distributed resources shared by collection of users from one or more organizations typically to work on common research goal
            * Provides conceptual framework for rules and regulations for resources to be offered/shared between VO institutions/members
            * Different domains place greater/lesser emphasis on expression and enforcement of rules and regulations
        > Example of Virtual Organization:
        >><img src='Virtual_Organization.png' width='50%'>
    * Many Technologies:
        * <font color=red>XACML, PERMIS, CAS, VOMS, AKENTI, VOMS, SAML, WS-*</font>
    * RBAC is a typical model
        * Basic Idea:
            * <font color=red>roles</font> applicable to specific collaboration
                * roles are often hierarchical
            * <font color=red>actions</font> allowed/not allowed for VO members
            * <font color=red>resources</font>comprising VO infrastructure
        * A policy then consists of sets of these rules
        * Policy engines consume this information to make access decisions
### Other Cloud Security Challenges
* Single sign-on
    * The Grid Model needed
    * Currently not solved for Cloud-based IaaS
    * Onus is on non-Cloud developers to define/support this
* Auditing
    * Logging, intrusion detection, auditing of security in external computer facilities
        * Well established in theory and practice and for local systems
        * Tools to support generation of diagnostic trails
* Deletion and encryption
    * Data deletion with no direct hard disk
        * Many tools and utilities do not work
    * Scale of data
* Liability
* Licensing
    * Many license models
    * Challenges with the Cloud delivery model
* Workflows
    * Many workflows tools for combing SoA services/data flows
    * Many workflows models
    * Serious challenges of:
        * Defining
        * Enforcing
        * Sharing
        * Enacting
    * Security-oriented workflows

