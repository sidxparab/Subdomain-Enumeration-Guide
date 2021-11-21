# TLS, CSP, CNAME Probing

## 1) TLS Probing

Nowadays generally all websites use HTTPS(HyperText Transfer Protocol Secure). In order to use HTTPS, the website owner needs to issue an SSL(Secure Socket Layer) certificate.

This SSL/TLS(Transport Layer Security) certificate sometimes contains domains/subdomains belonging to the same organization.

Clicking on the "Lock🔒" button in the address bar, you can view the TLS/SSL certificate of any website.

![Hackerone.com contain these subdomains in its TLS certificate](../../.gitbook/assets/TLS.png)



```bash
cat subdomains.txt | httpx -tls-probe -random-agent -status-code -retries 2 -no-color | anew tls_probed.txt | cut -d ' ' -f1 | unfurl -u domains | anew -q tls_subdomains.txt
```



![](<../../.gitbook/assets/tls probing.png>)



## 2) CSP Probing





```
cat subdomains.txt | httpx -csp-probe -random-agent -status-code -retries 2 -no-color | anew csp_probed.txt | cut -d ' ' -f1 | unfurl -u domains | anew -q csp_subdomains.txt
```

![](../../.gitbook/assets/csp.png)



