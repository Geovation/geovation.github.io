---
title: "HTTP headers: The bare-knuckle fight of application security"
tags: "Cyber Security"
meta: "The benefits of implementing HTTP security headers"
author: "Aymar Bell"
comments: true
---

## Introduction
My name is Aymar. I work for Geovation as a Junior software developer, with a focus on cyber security. I have a master's in Cyber security and recently qualified as a Certified Ethical Hacker from the [E-C Council][ecc]. My interests range from tennis, football and to all things related to Cybersecurity.

Security is a huge topic. Although I am reducing the scope of this article to web applications HTTP security headers, it's still nowhere near the comprehensive catalogue of concepts you need to know and consider. But I will attempt to highlight some basic good practices which every developer should carry out as a matter of course.    

HTTP security headers provide another layer of security by helping to mitigate attacks and web vulnerabilities. These headers tell your browser how to behave when handling your website's content.

In this article written in a personal capacity I will:

- point out common areas of vulnerabilities in a web application I have come across testing applications that developers need to be particularly aware of  

- provide reference and guidance in how to address each risk highlighted

## Definitions  
HTTP headers let the client and the server pass additional information with an HTTP request or response. An [HTTP header][http] consists of its name followed by a colon (:), then by its value.

## Headers categories  
The headers are grouped in four main subsets:

- General headers apply to both requests and responses, but with no relation to the data transmitted in the body.

- Request headers contain more information about the resource to be fetched, or about the client requesting the resource.

- Response headers hold additional information about the response, like its location or about the server providing it.

- Entity headers contain information about the body of the resource, like its content length or MIME type.  

## Case studies
Below are some of the web securities issues related to security headers that I have encountered whilst testing applications.

### 1. The Content-Security-Policy (CSP)

The HTTP Content-Security-Policy (CSP) response header allows web site administrators to control resources the user agent is allowed to load for a given page. The policies mostly involve specifying server origins and script endpoints.  

#### Threat mitigation
The CSP helps prevent attacks such as Cross Site Scripting (XSS) and other code injection attacks.  

#### Configuration
A CSP format is defined in the following way -  Content-Security-Policy: policy. For e.g., the following configuration ```Content-Security-Policy: default-src 'none'; script-src https://www.lognuk.co.uk``` tells the browser not to load any resources from any sources and the second part ```script-src https://www.lognuk.co.uk``` tells the browser to allow scripts from www.lognuk.co.uk over https://

#### Testing CSP
It’s always a good thing to test a configured CSP, therefore you should use [Content-Security-Policy-Report-Only][csp] instead.

### 2. The Expect-CT header  
This header lets sites opt in to reporting and/or enforcement of Certificate Transparency (CT) requirements. [Certificate Transparency][ct] is an open framework for monitoring SSL Certificates. With CT, all certificates are publicly disclosed. It makes it very difficult for a Certification Authority to issue an SSL/TLS Certificate for a domain without the certificate being visible to the owner of that domain. When this header is enabled the website is requesting the browser to verify whether or not the certificate appears in the public [CT logs][ctlogs].

#### Threat mitigation
- By setting the Expect-CT header, you can prevent mis-issued certificates to be used.

- Another reason to set it is that if [Google][report] runs into a certificate that is not seen in Certificate Transparency (CT) Log, it will consider that certificate invalid and reject the connection.

#### Configuration
The header could be defined as follows: ```Expect-CT: max-age=7776000, enforce, report-uri="https://www.xyz.com/report"```

### 3. X-Powered-By
This may be set by hosting environments or other frameworks and contains information about them while not providing any usefulness to the application or its visitors.

Here is an example of this: ```X-Powered-By: PHP/5.4.4```

An attacker can use this information to pull down the list of [CVEs][cves]. In this case, the total number of vulnerabilities for this version is __57__ as I am publishing this article. Using this information, the attacker can potentially exploit a server.

#### Threat mitigation

It is wise to unset this header to avoid exposing potential vulnerabilities or at the very least not mention the technology version of the application. The harder an attacker must work to identify your system’s technology, the more detectable their actions will be.

### 4. X-Frame-Options
The X-Frame-Options HTTP header field indicates a policy that specifies whether the browser should render the transmitted resource within a frame or an iframe. If this X-Frame-Options header is not included in the HTTP response from the server, It leaves the application open to 'ClickJacking' attacks.

#### Threat mitigation
Clickjacking is when an attacker uses multiple transparent or opaque layers to trick a user into clicking on a button or link on a framed page when they were intending to click on the top level page.
- Servers can declare this policy in the header of their HTTP responses, which ensures that their content is not embedded into other pages or frames.

#### Configuration
These could be instructions to the browser to not allow framing from other domains.
- ```X-Frame-Options: ALLOW-FROM URL``` - grants a specific URL to load itself in a iframe.
- ```X-Frame-Options: DENY``` - completely denies to be loaded in frame or iframe.
- ```X-Frame-Options: SAMEORIGIN``` - allows only if the site which wants to load has a same origin.

### 5. Protecting Data in Transit with HTTP Strict Transport Security (HSTS)

When using an ordinary HTTP connection, users are exposed too many risks because the data is transmitted in plaintext. An attacker capable of intercepting network traffic anywhere between a user's browser and a server can eavesdrop or even tamper with the data completely undetected, which compromises confidentiality and data integrity.

The good news is, we can protect against many of these risks with HTTPS or the even powerful security feature called HSTS. Most modern browsers support HSTS (HTTP Strict Transport Security), which allows a website to request that a browser only interact with it over HTTPS.

#### Threat mitigation

The HSTS's strict [rules][rules] mean that the following threats and the scenario above are addressed:

- Attackers using an invalid certificate in the hopes the user will accept the bad certificate.

- Old bookmarks that contain http:// or manually entered http:// URLs that can be vulnerable to an attack.

- Sites claiming to be fully HTTPS serving content over HTTP or containing HTTP links.

#### Configuration  

In order to enable this header, it is as simple as adding the following policy: ```Strict-Transport-Security: max-age=15768000```

## Conclusion
Your security posture is only as strong as the last test or measures implemented. The word I often hear is, you highlighted it as good practices therefore It is not important.

It's worth stating that what is considered good practices or a low vulnerability today could become a high security exploit further down the line. So, it's better to err on the side of caution and implement them, plus all major modern browsers currently support it.  

The following [API][api] is useful and allows you to retrieve which HTTP security headers that are currently running on your website.

[ecc]: https://www.eccouncil.org/
[http]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#Security
[csp]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
[ct]: https://www.certificate-transparency.org/
[ctlogs]: https://www.certificate-transparency.org/known-logs
[report]: https://transparencyreport.google.com/https/certificates?hl=en_GB
[cves]: https://www.cvedetails.com/vulnerability-list/vendor_id-74/product_id-128/version_id-130366/PHP-PHP-5.4.4.html
[rules]: https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html
[api]: https://securityheaders.com
