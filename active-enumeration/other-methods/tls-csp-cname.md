# TLS, CSP, CNAME Probing

## 1) TLS Probing

Nowadays generally all websites use HTTPS(HyperText Transfer Protocol Secure). In order to use HTTPS, the website owner needs to issue an SSL(Secure Socket Layer) certificate.

This SSL/TLS(Transport Layer Security) certificate sometimes contains domains/subdomains belonging to the same organization.

Clicking on the "Lock🔒" button in the address bar, you can view the TLS/SSL certificate of any website.

![Hackerone.com contain these subdomains in its TLS certificate](../../.gitbook/assets/TLS.png)



```bash
cat subdomains.txt | httpx -tls-probe -status-code -retries 2 -no-color | anew tls_probed.txt | cut -d ' ' -f1 | unfurl -u domains | anew -q tls_subdomains.txt
```



![](<../../.gitbook/assets/tls probing.png>)



## 2) CSP Probing

In order to defend from the XSS attacks as well as keeping in mind to allow cross-domain resource sharing in websites CSP(Content Security Policies) are used. These CSP headers sometimes contain domains/subdomains from where the content is usually imported.

Hence, these subdomains can be helpful for us. In the below image we can see I extracted domains/subdomains from the CSP header of [twitter.com](https://twitter.com)

```
cat subdomains.txt | httpx -csp-probe -status-code -retries 2 -no-color | anew csp_probed.txt | cut -d ' ' -f1 | unfurl -u domains | anew -q csp_subdomains.txt
```

![](../../.gitbook/assets/csp.png)
