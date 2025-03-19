# Passive Sources

### What is passive subdomain enumeration?

Passive subdomain enumeration is a technique to query for passive DNS datasets provided by services like [SecurityTrails](https://securitytrails.com/), [Censys](https://censys.io/), [Shodan](https://www.shodan.io/), [BinaryEdge](https://www.binaryedge.io/), [VirusTotal](https://www.virustotal.com/gui/), [Whoisxmlapi](https://main.whoisxmlapi.com/), etc. to obtain the subdomains of a particular target. Here we don't send any active probes to our target, instead passively try to scrape information available from the internet.

There are in total around[ **90** **passive DNS sources/services**](https://gist.github.com/sidxparab/22c54fd0b64492b6ae3224db8c706228) that provide such datasets to query them. It's difficult to manually query these third-party services thus, to ease this process various tools are developed which automate these processes.

{% hint style="warning" %}
It's highly recommended to read [**this**](https://app.gitbook.com/@sidxparab/s/subdomain-enumeration-guide/introduction/prequisites#what-is-passive-dns-data) section first, before proceeding further.
{% endhint %}

1. **Passive DNS enumeration tools**
   * [Amass](https://github.com/OWASP/Amass)
   * [Subfinder](https://github.com/projectdiscovery/subfinder)
   * [Assetfinder](https://github.com/tomnomnom/assetfinder)
   * [Findomain](https://github.com/Findomain/Findomain)
2. **Internet Archive**
   * [gau](https://github.com/lc/gau)
   * [waybackurls](https://github.com/tomnomnom/waybackurls)
3. **Github Scraping**
   * [github-subdomains](https://github.com/gwen001/github-subdomains)
4. **GitLab Scraping**
   * [gitlab-subdomains](https://github.com/gwen001/gitlab-subdomains)





## <mark style="color:orange;">A) Passive DNS gathering tools</mark>&#x20;

### <mark style="background-color:blue;">1) Amass</mark>

* **Author:** [OWASP](https://github.com/OWASP) (mainly [caffix](https://github.com/caffix)).
* **Language**: Go
* **Total Passive Sources**: **82**&#x20;

[**Amass** ](https://github.com/owasp-amass/amass)is a Swiss army knife for subdomains enumeration that outperforms passive enumeration the best. Amass queries the most number of third-party services which results in more subdomains of a particular target. [**These**](https://gist.github.com/sidxparab/e625a264322e4c9db3c3f1844b4a00b6) are passive services that amass queries.

### :gear: Configuring amass:

* Since amass written in Go, you need your Go environment properly set up([Steps](https://gist.github.com/sidxparab/e3856c5e27b8a9b27b5b4911eb9e4ae6) to setup Go environment)

**Installation:**

```bash
go install -v github.com/owasp-amass/amass/v3/...@master
```

**Setting up Amass config file:**

* [**Link**](https://gist.github.com/sidxparab/b4ffb99c98136dc4a238cbb88a77f642) to my amass config file for reference.
* To make it possible for Amass to query the passive DNS datasets, it necessary for us to setup the API keys of those services in the Amass configuration file.
* By default, amass config file is located at `$HOME/.config/amass/config.ini`&#x20;

{% hint style="info" %}
To get to know to create API keys, check out [**this article**](https://dhiyaneshgeek.github.io/bug/bounty/2020/02/06/recon-with-me/)**.**
{% endhint %}

* Now let's set up our API keys in the `config.ini`config file.
* Open the config file in a text editor and then uncomment the required lines and add your API keys.
* Refer to [my config file](https://gist.github.com/sidxparab/b4ffb99c98136dc4a238cbb88a77f642)(this is exactly how your amass config file should be)

<pre class="language-bash"><code class="lang-bash"><strong># https://otx.alienvault.com (Free)
</strong>[data_sources.AlienVault]
[data_sources.AlienVault.Credentials]
apikey = dca0d4d692a6fd757107333d43d5f284f9a38f245d267b1cd72b4c5c6d5c31


<strong>#How to Add 2 API keys for a single service
</strong><strong># https://app.binaryedge.com (Free)
</strong>[data_sources.BinaryEdge]
ttl = 10080
[data_sources.BinaryEdge.account1]
apikey = d749e0d3-ff9e-gcd0-a913-b5e62f6f216a
[data_sources.BinaryEdge.account2]
apikey = afdb97ff-t65e-r47f-bba7-c51dc5d83347
</code></pre>

### **Running Amass:**

* After setting up API keys now we are good to run amass.&#x20;

```bash
amass enum -passive -d example.com -config config.ini -o output.txt
```

**Flags:-**

* **enum** - Perform DNS enumeration
* **passive** - passively collect information through the data sources mentioned in the config file.
* **config** - Specify the location of your config file (default: `$HOME/.config/amass/config.ini` )
* **o** - Output filename

{% hint style="success" %}
:man\_mage:**Tip**: After configuring your config file in order to verify whether the API keys have been correctly set up or not you can use this command:\
&#xNAN;_&#x61;mass enum -list -config config.ini_
{% endhint %}

###

### <mark style="background-color:blue;">2) Subfinder</mark>

* **Author**: [projectdiscovery](https://github.com/projectdiscovery)
* **Language**: Go
* **Total Passive Sources**: **38**

[**Subfinder** ](https://github.com/projectdiscovery/subfinder)is yet another great tool that one should have in their pipeline. There are some unique sources that subfinder queries for, that amass doesn't. This tool is been developed by the famous ProjectDiscovery team, who's tools are used by every other bugbounty hunter.

### :gear:Configuring Subfinder: &#x20;

**Installation:**

```bash
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
```

**Setting up Subfinder configuration file:**&#x20;

* Subfinder's default config file location is at _`$HOME/.config/subfinder/provider-config.yaml`_&#x20;
* After your first installation, if you didn't find the configuration file populated by default run the following command again `subfinder` in order to get it generated.&#x20;
* The subfinder config file follows YAML(YAML Ain't Markup Language) syntax. So, you need to be careful that you don't break the syntax. It's better that you use a text editor and set up syntax highlighting.&#x20;

**Example config file:-**

* [**Link**](https://gist.github.com/sidxparab/ba50e138e5c912c7c59532ce38399d1b) to my subfinder config file for reference.
* Some passive sources like `Censys` , `PassiveTotal` use 2 keys in combination in order to authenticate a user. For such services, both values need to be mentioned with a colon(:) in between them. _(Check how have I mentioned the "Censys" source values- `APP-id`:`Secret` in the below example )_
* Subfinder automatically detects its config file only if at the default position.&#x20;

```yaml
securitytrails: []
censys:
  - ac244e2f-b635-4581-878a-33f4e79a2c13:dd510d6e-1b6e-4655-83f6-f347b363def9
shodan:
  - AAAAClP1bJJSRMEYJazgwhJKrggRwKA
github:
  - d23a554bbc1aabb208c9acfbd2dd41ce7fc9db39
  - asdsd54bbc1aabb208c9acfbd2dd41ce7fc9db39
passivetotal:
  - sample-email@user.com:password123
```

### **Running Subfinder:**

```bash
subfinder -d example.com -all -config config.yaml -o output.txt
```

**Flags:-**

* **d** - Specify our target domain
* **all** - Use all passive sources (slow enumeration but more results)
* **config** - Config file location

{% hint style="success" %}
:man\_mage: **Tip:-** To view the sources that require API keys `subfinder -ls` command
{% endhint %}

###

### <mark style="background-color:blue;">**3) Assetfinder**</mark>

* **Author**:  [tomnomnom](https://github.com/tomnomnom)
* **Language**: Go
* **Total passive sources**: **9**

Don't know why did I include this tool:joy:just because its build by the legend [Tomnomnom](https://twitter.com/TomNomNom) ?  It doesn't give any unique subdomains compared to other tools but it's extremely fast.

```bash
go install github.com/tomnomnom/assetfinder@latest
```

**Running:**

```bash
assetfinder --subs-only example.com > output.txt
```

###

### <mark style="background-color:blue;">4) Findomain</mark>

* **Author**: [Edu4rdSHL](https://github.com/Edu4rdSHL)
* **Language**: Rust
* **Total Passive sources**: 21

[**Findomain** ](https://github.com/Findomain/Findomain)is one of the standard subdomain finder tools in the industry. Another extremely fast enumeration tool. It also has a paid version that offers much more features like subdomain monitoring, resolution, less resource consumption.&#x20;

### Configuring Findomain: :gear:&#x20;

**Installation:-**

* Depending on your architecture download binary from [here](https://github.com/Findomain/Findomain/wiki/Installation#using-upstream-precompiled-binaries)

```bash
wget -N -c https://github.com/Findomain/Findomain/releases/download/9.0.0/findomain-linux.zip
unzip findomain-linux.zip
mv findomain /usr/local/bin/findomain
chmod 755 /usr/local/bin/findomain
```

**Configuration:-**

* You need to define API keys in your `.bashrc` or `.zshrc` .
* &#x20;Findomain will pick up them automatically.&#x20;

```bash
export findomain_virustotal_token="API_KEY"
export findomain_fb_token="API_KEY"
```

### **Running Findomain:**

```bash
findomain -t example.com -u output.txt
```

**Flags:-**

* **t** - Target domain
* **u** - Output file





## <mark style="color:orange;">B) Internet Archives</mark>

Internet Archives deploy their own web crawlers and indexing systems that crawl each website on the internet. Hence, they have historical data of all the websites that once existed. hence, Internet Archives can be a useful source to grab subdomains of a particular target that once existed and later perform permutations(more on this later) on them to get more valid subdomains.

Internet Archive when queried gives back URLs. Since we are only concerned with the subdomains, we need to process those URLs to grab only unique FQDN subdomains from them. &#x20;

For this, we use a tool called [unfurl](https://github.com/tomnomnom/unfurl). This tool helps to extract the domain name from a list of URLs.&#x20;

### <mark style="background-color:blue;">5) Gau</mark>

* **Author**: [lc](https://github.com/lc)
* **Language**: Go
* **Sources**:
  * [web.archive.org](http://web.archive.org/)
  * [index.commoncrawl.org](http://index.commoncrawl.org/)
  * [otx.alienvault.com](https://otx.alienvault.com/)
  * [urlscan.io](https://urlscan.io/)

[**Gau** ](https://github.com/lc/gau)works by querying all the above 4 internet archive services and grabs all the URLs that their internet-wide crawler had once crawled. So through this process we get tons of URL's belonging to our target that once existed. After collecting the URLs we extract only the domain/subdomain part from those URLs.

#### Installation:

```bash
go install github.com/lc/gau/v2/cmd/gau@latest
```

#### Running gauplus:

```bash
gau --threads 5 --subs example.com |  unfurl -u domains | sort -u -o output_unfurl.txt
```

**Flags:**

* **threads** - How many workers to spawn
* **subs** -  Include subdomains of the target domain



### <mark style="background-color:blue;">**6) Waybackurls**</mark>

* **Author**: [tomnomnom](https://github.com/tomnomnom)
* **Language**: Go
* **Sources**:
  * [web.archive.org](http://web.archive.org/)
  * [index.commoncrawl.org](http://index.commoncrawl.org/)
  * [www.virustotal.com](https://www.virustotal.com)

[**Waybackurls**](https://github.com/tomnomnom/waybackurls) works similar to Gau, but I have found that it returns some unique data that Gau couldn't find. Hence, we need to include waybackurls in our arsenal.

#### Installation:

```bash
go install github.com/tomnomnom/waybackurls@latest
```

#### &#x20;**Running Waybackurls:**

```bash
waybackurls example.com |  unfurl -u domains | sort -u -o output.txt
```







## <mark style="color:orange;">C) GitHub Scraping</mark>

### <mark style="background-color:blue;">7) Github-subdomains</mark>

* **Author**: [gwen001](https://github.com/gwen001)
* **Language**: Go

Organizations sometimes host their source code on GitHub, also employees working at these organizations sometimes leak the source code on GitHub. Additionally, I have came around instances where security researchers host their reconnaissance data in public repositories. The tool Github-subdomains can help you extract these exposed/leaked subdomains of your target from GitHub.

**Installation:**

```bash
go install github.com/gwen001/github-subdomains@latest
```

:gear:**Configuring github-subdomains​​:**&#x20;

* For github-subdomains to scrap domains from GitHub you need to specify a list of GitHub access tokens.
* [**Here**](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic) is an article on how you can generate your GitHub access tokens.
* These access tokens are used by the tool to perform searches and find subdomains on behalf of you.
* I always prefer that you make at least 10 tokens from 3 different accounts(30 in total) to avoid rate limiting.&#x20;
* Specify 1 token per line.

**Running github-subdomains:**

```bash
github-subdomains -d example.com -t tokens.txt -o output.txt
```

**Flags:**

* **d -** target
* **t** - file containing tokens
* **o** - output file



## ~~**D)** Rapid7 Project Sonar dataset(depreciated)~~

[Project Sonar](https://opendata.rapid7.com/about/) is a security research project by Rapid7 that conducts internet-wide scans. Rapid7 has been generous and made this data freely available to the public. Project Sonar contains [8 different datasets](https://opendata.rapid7.com/) with a total size of over **66.6 TB** which are updated on a regular basis. You can read here how you can parse these datasets on your own using this [guide](https://0xpatrik.com/project-sonar-guide/).&#x20;

This internet-wide DNS dataset could be an excellent resource for us to grab our subdomains right? But querying such large datasets could take up significant time. That's when **Crobat** comes to the rescue.

### 8) [Crobat](https://github.com/Cgboal/SonarSearch)

* **Author**: [Cgboal](https://github.com/Cgboal)
* **Language**: Go

[Cgboal ](https://twitter.com/CalumBoal)has done an excellent work of parsing and indexing the whole Rapid7 Sonar dataset into MongoDB and creating an API to query this database. This Crobat API is freely available at [https://sonar.omnisint.io/](https://sonar.omnisint.io/).More over he developed a command-line tool that uses this API and returns the results at a blazing fast speed.

### Installation:

```bash
go get github.com/cgboal/sonarsearch/cmd/crobat
```

### Running:

```bash
crobat -s example.com > output.txt
```

**Flags:**

* **s** - Target Name







## :checkered\_flag:**That's it !!! Done with passive things** :checkered\_flag:&#x20;

#### Liked my work? Don't hesitate to buy me a coffee XDD

#### :heart::blue\_heart::green\_heart: [https://www.buymeacoffee.com/siddheshparab](https://www.buymeacoffee.com/siddheshparab) :green\_heart: :blue\_heart: :heart:&#x20;

