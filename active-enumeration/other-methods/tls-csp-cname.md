# TLS, CSP, CNAME Probing

## 1) TLS Probing

TLS(Transport Layer Security)







```bash
cat subdomains.txt | httpx -tls-probe -random-agent -status-code -retries 2 -no-color | anew tls_probed.txt | cut -d ' ' -f1 | unfurl -u domains | anew -q tls_subdomains.txt
```



![](<../../.gitbook/assets/tls probing.png>)
