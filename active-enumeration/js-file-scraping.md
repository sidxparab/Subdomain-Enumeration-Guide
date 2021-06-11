# Scraping\(JS/Source code\)





Javascript files are used by modern web applications providing dynamic content which contains various functions & events. Each website's processes its Javascript files are a great resource for finding those internal subdomains used by the organization. Mostly JS files 





## Tools:

### 1\) Gopsider

* Author: [Jaeles](https://github.com/jaeles-project)
* Language: Go

[**Gospider**](https://github.com/jaeles-project/gospider) is a fast web spidering tool capable of crawling the whole website within in a short amount of time. This means gospider will visit/scrap each and every URL mentioned in the JS file and source code. So, since source code & JS files makeup a website they may contain links to other subdomains too. 

### Installation:

```bash
go get -u github.com/jaeles-project/gospider
```

### Running:

* Since we are crawling a website, gospider excepts us to provide URL's, means in the form of `http://`  `https://` 
* So, first we need to web probe all the subdomains we have gathered till now. For this purpose we will use [**httpx**](https://github.com/projectdiscovery/httpx) .
* So, lets first web probe the subdomains:

```bash
cat subdomains.txt | httpx -follow-host-redirects -random-agent -retries 2 -no-color -o tmp_output.txt
cat tmp_output.txt | cut -d ' ' -f1 | tee output.txt
```

```bash
gospider -S probed_tmp_scrap.txt --js -t 150 -d 3 --sitemap --robots -w -r > gospider.txt
```

