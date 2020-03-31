---
title: "Why is ethical hacking relevant for start-ups?"
tags: "Cyber Security"
meta: "An explanation of the benefits of Web Application testing for the start-ups"
author: "Aymar Bell"
comments: true
---

## Introduction

My name is Aymar. I work for Geovation as a Junior software developer, latterly with a focus on security. I have a master's in security and about to complete the E-C Council certification as an Ethical Hacker. My interests range from tennis, football and to all things related to Cybersecurity.   

As Sun Tzu states in the Art of War:
> "If you know yourself but not the enemy, for every victory gained, you will also suffer a defeat".

No company big or small is immune from cyberattacks. The number and type of cyberattacks grows every day. According to [CisoMag][cm], small businesses in the UK suffer 10,000 cyber-attacks per day. Despite this explosion of vulnerabilities, a significant percentage of cyberattacks are predictable and preventable.  

Cyber incidents can have financial, legal and reputational impact. The costs may include forensic investigations, legal fees and technology changes. It is every company responsibility to guard its business against ill-intended users, and therefore should take good steps to secure data especially where those steps are simple.

Often the question for start-up is where to start and in extreme cases why I should care. The aim of this article is to explain the benefits of web application security as it demonstrates the start-up's commitment to cyber security and why Geovation Hub members should see the security of the web application as the glue that holds their start-up together.  

# Some useful definitions

Acunetix’s Web Application Vulnerability [Report][Acunetix] 2019 reports that websites have 46% high and 87% medium security vulnerabilities.  

Web applications are software programs that run on web browsers and act as the interface between users and web servers through web pages. They help the users request, submit, and retrieve data to/from a database over the Internet by interacting through a user-friendly graphical user interface (GUI).  The common methods of attacks against web applications range from database manipulation to wide-scale network disruption.

- SQL injection – It is a method used by attackers to exploit vulnerabilities in the way a database executes search queries. Attackers can then gain access to unauthorized information, modify or create user permissions, manipulate or destroy sensitive data.

- Social engineering is an art of manipulating people to divulge sensitive information to perform some malicious action.

- Cross-site request forgery (CSRF) involves tricking a victim into making a request that utilizes their authentication or authorization. The attacker is then able to send a request masquerading as the user.  

- Denial-of-Service (DoS) and Distributed Denial-of-Service (DDoS) attacks attempt to make a victim’s website or network resource unavailable to its authorized users by flooding it with nonlegitimate service requests or traffic to overload its resources, bringing the system down, or at least significantly slowing the victim’s system or network performance.  

- Cross site scripting (XSS) - It is a vulnerability that allows an attacker to inject client-side scripts into a webpage in order to access important information directly, impersonate the user, or trick the user into revealing important information.

# The start-up reality

The life of a start-up in its early days is about securing that initial investment. Having spoken to quite a few of founders, it can be a challenging period.  A cyber breach during that period of the start-up’s life would not help matters, as this will prompt even the keenest investor to raise uncomfortable questions.

## Carry/Bring your own device (BYOD)

This approach is now the norm for start-ups and entrepreneurs, as it brings down the expenditure and working on a familiar personal device has been associated with an increase in productivity. It can also minimise overheads for the start-up relating to procurement and provisioning. At the same time, BYOD can be a real challenge when it comes to security.

## Is your start-up ready against web attacks?

There’s no silver bullet. The theory suggests that, a developer that applies secure coding practices by using thorough input/output sanitization could eliminate all vulnerabilities, making an application immune to malicious manipulation.

### web application firewall (WAF)

Another solution to address the attacks related to web applications is to use a WAF. A web application firewall helps protect a web application against malicious HTTP traffic. It works by placing one between the company server and the potential threats to the web application. The WAF can protect against attacks like cross site forgery, cross site scripting, SQL injection, and can help to meet criteria for compliance certification. Examples of Web Application Firewall include [CloudFlare’s WAF][cwaf], [Amazon AWS WAF][awaf].

# What is Hacking?  

Three-quarters of network penetration vectors resulted from poor web application security, according to a 2019 report on vulnerabilities in corporate information systems by [Positive Technologies][pts].  

Hacking in the field of computer security refers to exploiting system vulnerabilities and compromising security controls to gain unauthorized or inappropriate access to system resources. It involves modifying system or application features to achieve a goal outside its creator's original purpose.

## What are Hackers testing?

Web applications are an integral component of online business. However, they are also vulnerable to more sophisticated threats and attack vectors. In order to assess organization’s security against web-application attacks, hackers use a methodology that allows them to plan each step in detail to increase their chances of successfully hacking the applications.

There are various methodologies which are used as a reference for performing an attack, namely [PTES][ptes] (Penetration Testing Execution Standard), [OWASP][owasp] (Open Web Application Security Protocol) and [OSSTMM][osstmm] (Open Source Security Testing Methodology Manual) – The major points are listed below.

- Analysing web applications

  - What is the purpose of the web application?

  - Who is the audience of the web application?

  - How critical is the web application from a business perspective?

  - What technology platform has been used to develop the web application?

- Identifying the web application’s entry points, for e.g. a login screen or a registration form.

- Testing for vulnerabilities using several tools, including proprietary and open sources. This involves intercepting potential HTTP requests, comprehensive scans, modifying and tampering with the parameter values, and then analysing the web application’s response.

- Lastly but not least, we must produce a report with all the vulnerabilities identified and, in some cases, suggest a countermeasure for each identified vulnerability.

# Case Studies

Start-ups are at different stages of development. At Geovation, we emphasize the risks of not being cyber aware, and Start-ups can now receive a range of Cyber Security advice and support in issues related but not limited to the following:

1. Weak authentication mechanisms (weak passwords, account lockout) and inherent limitations of one-factor authentication mechanisms, which allow attacker to gain unauthorized access – It addresses the default security implemented by developers  

2. How to implement a basic cyber hygiene leading up to the Cyber Essentials Certification – It shows the commitment to security. For e.g. one start-up is currently enjoying the benefits of this exercise.  

3. Web application security assessment – It helps to protect against unskilled internet-based attackers using tools which are freely available on the internet. It also adds value to the start-up web solution and can be the difference to get a foot in an investor’s door to secure investment.

# Conclusion

Start-ups face many challenges, and cyber security should not be the one that is addressed at the last minute. The harsh truth is that hackers are always on the lookout for the path of least resistance.  

This means the IT security of a start-up shouldn’t just be the priority of the most IT conscious member of the business. It must permeate every aspect of the start-up. From the founder to the new intern, the structure of the start-up should ensure every employee has the basic IT skills such as password management or education on how to identify phishing scams.  

At the heart of it all, every start-up should have an individual who is responsible for managing the information systems and this person must be a member of the organisation. This is vital and could save the start-up thousands of pounds.

[cm]: https://www.cisomag.com/small-businesses-in-the-uk-suffer-10000-cyber-attacks-per-day-fsb/
[Acunetix]: https://www.cisomag.com/small-businesses-in-the-uk-suffer-10000-cyber-attacks-per-day-fsb/
[pts]: https://www.ptsecurity.com/ww-en/analytics/corp-vulnerabilities-2019/
[cwaf]: https://www.cloudflare.com/en-gb/waf/
[awaf]: https://aws.amazon.com/waf/
[ptes]: http://www.pentest-standard.org/index.php/Main_Page
[owasp]: https://owasp.org/www-project-top-ten/
[osstmm]: https://www.isecom.org/OSSTMM.3.pdf
