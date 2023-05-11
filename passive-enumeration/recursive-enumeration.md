# Recursive Enumeration

Through various testing and trying out new things for subdomain enumeration, my friend [Six2dez ](https://twitter.com/Six2dez1)came across a technique where running the subdomain enumeration tools again on each of the subdomains found yields in getting more subdomains in total

In easy words, we again run tools like Amass, Subfinder, Assetfinder again each of the subdomains that were found.

{% hint style="danger" %}
If you have set up API keys, this technique may consume your entire querying quota.
{% endhint %}

For a better understanding look at the image below.

![](<../.gitbook/assets/Recursive Enumeration.png>)

Run the below script on the subdomains that you have already found.

_Replace `subdomains.txt` with the filename of your subdomains list._

```bash
for sub in $( ( cat subdomains.txt | rev | cut -d '.' -f 3,2,1 | rev | sort | uniq -c | sort -nr | grep -v '1 ' | head -n 10 && cat subdomains.txt | rev | cut -d '.' -f 4,3,2,1 | rev | sort | uniq -c | sort -nr | grep -v '1 ' | head -n 10 ) | sed -e 's/^[[:space:]]*//' | cut -d ' ' -f 2);do 
    subfinder -d example.com-all -silent | anew -q passive_recursive.txt
    assetfinder --subs-only example.com | anew -q passive_recursive.txt
    amass enum -passive -d example.com | anew -q passive_recursive.txt
    findomain --quiet -t example | anew -q passive_recursive.txt
done
```
