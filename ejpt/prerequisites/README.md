---
description: Technical aspects of systems, networks, and applications.
---

# Penetration Testing Prerequisites

* A penetration tester performs a deep investigation of a remote system's security flaws.
* Penetration testers must test for any and all vulnerabilities, not just the ones that may grant them root access.
* Penetration testing is not about getting `root`!
* Penetration testers cannot destroy their client's infrastructure, professional pentesting requires a thorough understanding of attack vectors and their potential.
* Pentester activity must guarantee that the least impact possible on the production systems and services.
* Avoid overloading client's systems and networks.
* Communicate to client what steps to take, just in case anything goes wrong during the pentest.
* Pentesting is a process that ensures that every potential vulnerability or security weakness gets tested with the lowest possible overhead.

## Phase #0: Engagement

Details about the pentest are established during the Engagement phase.

Quotation:

* Fee establishment for the job to be accomplished.
* Fee will vary according to:
  * Type of engagement: black box, gray box, etc.
  * How time-consuming the engagement is.
  * The complexity of the applications and services in scope.
  * The number of targets (IP addresses, domains, etc.)
* Evaluation and quoting these aspects requires experience.
* If you are not able to quantify the amount of work required by an engagement, you can provide an hourly fee.

### Proposal Submittal

* The best way to win a job is by providing a sound and targeted proposal.
* You should write the proposal keeping in mind the client's needs and infrastructure.
* Must include:
  * Your understanding of the customer's requirements.
  * Approach & Methodology
    * Automated scanning tools
    * Manual testing
    * Onsite testing
    * Any other
  * Value the pentest will bring to business
  * Risks & Benefits
    * Business continuity
    * Improved confidentiality
    * Avoidance of money and reputation loss
  * Estimate of the time / price
  * Type:
    * Penetration test
    * Vulnerability assessment
    * Remote / Onsite
  * Scope
    * IP addresses
    * Network blocks
    * Domain names
    * Etc.

### Scope

* Make sure that the target of your engagement is the property of your client.
* Shared hosting: You must not conduct an assessment on targets unless you are given written permission from the hosting provider.
* Check country laws

### Incident Handling

* Unplanned and unwanted situation that affects the client's environment and disrupts its services.
* Even when sticking to best practices,there's a possibility to damage the tested assets.
* Aim not to damage the target.
* In case of planning some insensitive or risk test, communicate with the customer.
* Have a 'Incident Handling Procedure':
  * Set of instructions that need to be executed by both you and your customer on how to proceed when a incident occurs.
  * Have an emergency contact.
  * Add an statement to the rules of engagement.

### Legal work

* Sometimes you will need to involve a lawyer as information security laws vary a lot from country to country
* Sometimes a professional insurance is required
* NDA can be signed. Keep data private and encrypted on your PC.
* Outline what you can and you cannot do.
* Rules of engagement is another document that will define the scope of engagement and will put on paper what you are entitled to do and when, this includes the time window for your tests and your contacts in the client's organizations

## Phase #1: Information Gathering

* Fundamental stage for a successful penetration test.
* Starts once the legal paperwork is complete, and not before.
* You investigate and harvest info about the client's company

Extremely useful information if Social Engineering is allowed by the rules of the engagement:

* Emails and addresses
  * Board of directors
  * Investors
  * Managers and employees
* Branch location and addresses
* Identify risks in the client's critical infrastructure.
* Having an understanding of the business is a key aspect in understanding what is important for your client.
* Understanding the business allows us to rate the risks associated with a successful attack.

### Infrastructure Information Gathering

* After understanding the business
* Transform the IP addresses or the domains in scope into actionable information about servers, OSs, etc.
* If the scope is defined as a list of IP addresses, you can move on to the next step.
* If the scope is the whole company or some of their domains, you will have to harvest the relevant IP blocks by using `WHOIS` and other _DNS information_.
* Give meaning to every IP address in scope, determining the following in order to focus your efforts/attacks and select the right tools for the exploitation phase:
  * if there's a live host or server using it
  * if there are one or more websites using that IP address
  * What OS is running on the host or the server

### Web Applications

* Harvest Domains
* Harvest subdomains
* Harvest Pages (website crawling)
* Harvest Technologies in use, like PHP, Java, .NET
* Harvest Frameworks and CMS in use
* Treat webapps as completely separate entities

## Phase #2: Footprinting and Scanning

Here you deepen your knowledge of the in-scope servers and services.

### Fingerprinting the OS

* Gives you info about the OS
* Helps to narrow down the number of potential vulnerabilities
* Some tools use exploits to some singularities you can find the network stack implementation

### Port Scanning

* Once you know the live hosts, you can determine which ports are open on a remote system
* Any mistake made here will impact next steps
* `nmap` uses different scanning techniques to reveal open, closed and filtered ports

### Detecting Services

* Act of knowing what service which service is listening on that port.
* Knowing the port number isn't enough, there's the need to discover the service that is running behind.
* `nmap` can be used to fingerprint ports.

By knowing the services running, we can know:

* OS
* Purpose of a particular IP address (server/client)
* Relevance of the host in the infrastructure

## Phase #3: Vulnerability Assessment

* Aims to build a list of the present vulnerabilities on the target systems.
* The pentester will carry out a vulnerability assessment on each target discovered in the previous steps
* The bigger the list, the more chances we'll have when exploiting the systems in scope.

Vulnerability assessments can be carried out:

* Manually
* Using automated tools, as scanners that will send probes to the target systems to detect whether a host has some well-known vulnerabilities
  * Extremely important to properly configure them, you might crash targets if not
  * Their output is a report that the pentester can use in the exploitation phase

## Phase #4: Exploitation

* Phase where we verify the vulnerabilities really exist.
* During this phase, a pentester checks and validates a vulnerability and also widens and increases the privileges on the target systems and networks.
* A penetration test is a cyclic process:
* The process finishes when there are no more systems and services in-scope to exploit.

{% hint style="info" %}
**Information Gathering**  [**→**](https://www.toptal.com/designers/htmlarrows/arrows/right-arrow/) **Scanning**  [**→**](https://www.toptal.com/designers/htmlarrows/arrows/right-arrow/) **Vulnerability Assessment**  [**→**](https://www.toptal.com/designers/htmlarrows/arrows/right-arrow/) **Exploiting**
{% endhint %}

## Phase #5: Reporting

* This step is as important as the rest of the phases, as it delivers the results to executives, IT staff and development team.
* This report must address:
  * Techniques used
  * Vulnerabilities found
  * Exploits used
  * Impact and risk analysis for each vulnerability
  * Remediation tips (of real value for the client, as they can be used to resolve their security issues)

## Consultancy

* Pentester are often asked to provide some hours of consultancy after delivering the report.
* The initial engagement is closed and the pentester must keep the report encrypted in a safe place, or even destroy it.
