# Recursive Enumeration

Through various testing and trying out new things for subdomain enumeration, my friend [Six2dez ](https://twitter.com/Six2dez1)came across a technique where running the subdomain enumeration tools again on each of the subdomains found yields in getting more subdomains in total

In easy words, we again run tools like Amass, Subfinder, Assetfinder again each of the subdomains that were found.

{% hint style="danger" %}
If you have set up API keys, this technique may consume your entire querying quota.
{% endhint %}

For a better understanding look at the image below:

![](<../.gitbook/assets/Recursive Enumeration.png>)

### Things to keep in mind:

* This technique is only useful when your target has a large number of multi-level subdomains_(not effective for small & medium scope targets)._
* This technique should be performed in the end and only on valid list of subdomains that you have gathered using other Passive + Active techniques.&#x20;
* This techniques takes time to return the final results.

### Workflow:

1. Read the list of subdomains from the file "subdomains.txt".
2.  Process the subdomains in two steps:

    a. Find the Top-10 most frequent occuring Second-Level domains(backup, dev,&#x20;





Run the below script on the subdomains that you have already found.

_Replace `subdomains.txt` with the filename of your subdomains list._

```bash
#!/bin/bash

go install -v github.com/tomnomnom/anew@latest
subdomain_list="subdomains.txt"

for sub in $( ( cat $subdomain_list | rev | cut -d '.' -f 3,2,1 | rev | sort | uniq -c | sort -nr | grep -v '1 ' | head -n 10 && cat subdomains.txt | rev | cut -d '.' -f 4,3,2,1 | rev | sort | uniq -c | sort -nr | grep -v '1 ' | head -n 10 ) | sed -e 's/^[[:space:]]*//' | cut -d ' ' -f 2);do 
    subfinder -d $sub -silent -max-time 2 | anew -q passive_recursive.txt
    assetfinder --subs-only $sub | anew -q passive_recursive.txt
    amass enum -timeout 2 -passive -d $sub | anew -q passive_recursive.txt
    findomain --quiet -t $sub | anew -q passive_recursive.txt
done
```
