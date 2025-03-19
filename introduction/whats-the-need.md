# What's the need ?🤔

## **What is subdomain enumeration?**

It is one of the most crucial parts of the reconnaissance phase while performing a security assessment. **Subdomain Enumeration** is a process of finding sub-domains associated to the root domain. According to [RFC 1034](https://tools.ietf.org/html/rfc1034), "_a domain is a subdomain of another domain if it is contained within that domain_".

![](../.gitbook/assets/subdomains.png)

## What's the need?

* Performing subdomain enumeration via various intensive techniques can help enlarge your attack surface, as you get more assets to find vulnerabilities on.
* A good subdomain enumeration will help you find those hidden/untouched subdomains, resulting lesser people finding bugs on that particular domain. Hence lesser **duplicates**.
* Finding applications running on hidden, forgotten(by the organization) sub-domains may lead to uncovering critical vulnerabilities.
* Discovering such strangely named subdomains is a critical skill, each bug hunter should possess in today's time.
*   For large organizations, to find what services have they exposed to the internet while performing an internal pentest.



{% hint style="success" %}
**More the subdomains = More assets to look for vulnerabilities**:lady\_beetle:&#x20;
{% endhint %}

## :warning: Common Misconception about "subdomain"&#x20;

A Fully Qualified Domain Name (**FQDN**) is the complete domain name for a specific computer, or host, on the internet.

An FQDN looks like this:-

`myhost.example.com.`  **---->** Fully Qualified Domain Name&#x20;

&#x20;`myhost` **---->** is the host located within the domain example.com (subdomain)



**Hence;**\
[**https://**&#x65;xample.com](https://example.com)\
[**http://**&#x6D;yhost.example.com](http://myhost.example.com)\
[**https://**&#x69;nternal.accounts.example.com  ](https://internal.accounts.example.com)\
[**http://**&#x69;nternal.accounts.dashboard.example.com](https://internal.accounts.dashboard.example.com)

The above-mentioned **cannot** be called as subdomains. They are the hyperlinks to web applications hosted the respective hosts. Most people have a misconception that these are subdomains of a particular target.

Let's consider an example, **`admin.example.com`**  is a subdomain on which there isn't any web service hosted. This means that, when we send web probes to `admin.example.com` using [httpx](https://github.com/projectdiscovery/httpx)/[httprobe ](https://github.com/tomnomnom/httprobe)**(**&#x74;ools that check whether any web service is running on that host), it will not return any output.

This doesn't mean that `admin.example.com` is not a valid subdomain of root domain `example.com`There may exists other services like SSH, SMTP, SMB, WinRM(non-web) hosted on that subdomain that cannot be accessed through your web browser. Surprisingly these services may be vulnerable and their exploits would be publicly available. \
\
So in such a case, it's always better that you **DNS resolve** the subdomains that are gathered from passive enumeration to get the valid ones. Later you can send the valid/alive subdomains for **web probing** and find out the hosted web applications on them.

### **Moral of the story:**

The methodology of collecting subdomains from tools like amass, subfinder, findomain and directly sending them to httpx/httprobe is **absolutely wrong**:x:. Instead, you should first DNS resolve them using tools like [puredns ](https://github.com/d3mondev/puredns)or [shuffledns](https://github.com/projectdiscovery/shuffledns).&#x20;

\


\




