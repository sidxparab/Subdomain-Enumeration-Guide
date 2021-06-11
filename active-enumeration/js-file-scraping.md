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

```bash
gospider -S probed_tmp_scrap.txt --js -t 150 -d 3 --sitemap --robots -w -r > gospider.txt
```

