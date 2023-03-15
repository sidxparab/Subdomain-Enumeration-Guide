# Horizontal Enumeration

While performing a security assessment our main goal is to map out all the root domains owned by a single entity. This means, making an inventory of all the internet facing assets of a particular organization. It is a bit trickier to find related domains/acquisitions of a particular organization as this step includes some tedious methods and doesn't guarantee accurate results always. One has to solely perform manual analysis to verify the results.

From the below image you can get an idea of what a **horizontal domain correlation** is:

![](../.gitbook/assets/enumeration-2-.png)

\
Let's look at how to find these related horizontal domains.

## Methods:

{% hint style="danger" %}
These enumeration methods can go out of scope and backfire you. Do it with caution!
{% endhint %}

### 1) Discovering the IP space

**ASN**(Autonomous System Number) is a unique identifier for a set of IP-ranges an organizations owns. Very large organizations such as Apple, GitHub, Tesla have their own significant IP space. To find an ASN of a particular organization, [https://bgp.he.net](https://bgp.he.net/) is a useful website where we can query.\
Let's find ASN for **Apple Inc.**

![](../.gitbook/assets/hurricane.png)

Now that we have found out the ASN number of an organization, the next step is to find out the IP ranges that reside inside that ASN. For this, we will use a tool called **whois.**

<pre class="language-bash"><code class="lang-bash">apt-get install whois
<strong>whois -h whois.radb.net  -- '-i origin AS714' | grep -Eo "([0-9.]+){4}/[0-9]+" | uniq -u
</strong></code></pre>

<figure><img src="../.gitbook/assets/whoiss.png" alt=""><figcaption></figcaption></figure>

### 2) Finding related domains/acquisitions

#### a) **WhoisXMLAPI**

****[**WhoisXMLAPI** ](https://www.whoisxmlapi.com/)is an excellent source that provides a good amount of related domains & acquisitions based on the WHOIS record. Singing up on their platform will assign you **500 free credits** which renew every month.\
Visit [https://tools.whoisxmlapi.com/reverse-whois-search](https://tools.whoisxmlapi.com/reverse-whois-search) . Now searching with the root domain name like **dell.com** will give all the associated domains.

![](../.gitbook/assets/whoisxml.png)

{% hint style="warning" %}
These are not 100% accurate results, as they contain false positives &#x20;
{% endhint %}

#### b) **Whoxy** :moneybag:&#x20;

****[**Whoxy**](https://www.whoxy.com/) is yet another great source to perform reverse WHOIS on parameters like Company Name, Registrant Email address, Owner Name. Whoxy has an enormous database of around **329M WHOIS records**. But sadly this is a paid service :(

To effectively use Whoxy API there's a command-line tool called [**whoxyrm**](https://github.com/MilindPurswani/whoxyrm)**.**

![](../.gitbook/assets/whoxyrm.png)

#### c) Crunchbase:moneybag:&#x20;

[**Crunchbase**](https://www.crunchbase.com/) is another great alternative for finding acquisitions but requires a paid subscription to view all the acquisitions. The trial version allows viewing some of the acquisitions.

![](../.gitbook/assets/crunchbase.png)

### 3) PTR records (Reverse DNS)

Now since we have got to know the IP address ranges from ASN of an organization, we can perform PTR queries on the IP addresses and check for valid hosts.\
\
**What is reverse DNS?**\
When a user attempts to reach a domain name in their browser, a DNS lookup occurs, matching the domain name(example.com) to the IP address(such as 192.168.1.1). A reverse DNS lookup is the opposite of this process: it is a query that starts with the IP address and looks up the domain name.

This means that, since we already know the IP space of an organization we can, we can reverse query the IP addresses and find the valid domains. Sounds cool?

**But how?**\
****PTR records (pointer record) helps us to achieve this. Using [**dnsx**](https://github.com/projectdiscovery/dnsx) **** tool we can query a PTR record of an IP address and find the associated hostname/domain name.

**Apple Inc.** :apple:  has **ASN714** which represents IP range **17.0.0.0/8.** So, let's see have to perform reverse DNS.

### Running:

We will first need to install 2 tools:

* [**Mapcidr**](https://github.com/projectdiscovery/mapcidr) **** :- `GO111MODULE=on go get -v github.com/projectdiscovery/mapcidr/cmd/mapcidr`
* ****[**dnsx** ](https://github.com/projectdiscovery/dnsx)       :- `GO111MODULE=on go get -v github.com/projectdiscovery/dnsx/cmd/dnsx`

```bash
 echo 17.0.0.0/8 | mapcidr -silent | dnsx -ptr -resp-only -o output.txt
```

#### Breakdown:

* When an IP range is given to **mapcidr** through stdin(standard input), it performs expansion spitting out each IP address from the range onto a new line:`17.0.0.1`**,** `17.0.0.2`**,** `17.0.0.3`**,** `17.0.0.4`
* Now when **dnsx** receives each IP address from stdin, it performs reverse DNS and checks for PTR record. If, found it gives us back the hostname/domain name.

![](../.gitbook/assets/ptr.png)

**Note:** We can also combine the step of discovering the IP space with reverse DNS lookup into one-liner like:

```bash
 whois -h whois.radb.net  -- '-i origin AS714' | grep -Eo "([0-9.]+){4}/[0-9]+" | uniq | mapcidr -silent | dnsx -ptr -resp-only
```

### 4) Favicon Hashing

#### What is a favicon?

The image/icon shown on the left-hand side of a tab is called as **favicon.ico**. This icon is generally fetched from a different source/CDN. Hence, we can find this favicon link from the source code of the website.

![](../.gitbook/assets/favicon.png)

#### How to find the favicon.ico link?

* Visit any website which already posses a favicon ([https://www.microsoft.com](https://www.microsoft.com/en-in))
* Now, view the source code and find the keyword "**favicon**" in the source code.
* You will find the link where the favicon is hosted. ([https://c.s-microsoft.com/favicon.ico](https://c.s-microsoft.com/favicon.ico?v2))

#### Generating the MurmurHash value:

To generate the MurmurHash value which is unique to each favicon we will use a tool called **MurMurHash**

### ****[**MurMurHash**](https://github.com/Viralmaniar/MurMurHash)****

* **Author**: [Viral Maniar](https://github.com/Viralmaniar)
* **Language**: Python

**MurMurHash** is a simple tool used to generate hash for the given favicon.

#### Installation:

```bash
git clone https://github.com/Viralmaniar/MurMurHash.git
cd MurMurHash/
pip3 install -r requirements.txt
```

#### Running:&#x20;

* Upon running the tool, it will ask you to enter the URL for the hash.
* And after entering the favicon link it will provide you with a unique hash value (**-2057558656**)&#x20;

```bash
python3 MurMurHash.py
```

![](../.gitbook/assets/favicontool.png)

### Weaponizing through Shodan:

* Now we query [Shodan](https://www.shodan.io/) `http.favicon.hash:<hash>` with that favicon hash.
* This gave us a whopping **162K assets/hosts**. These all can be subdomains or related domains of the Microsoft organization.

![](../.gitbook/assets/shodanfavicon.png)



**You know this is a powerful technique when the Recon king**:crown: **tweets about it.**

![](../.gitbook/assets/jhaddixtweet.png)

\
\


## &#x20;**** :checkered\_flag:**That's it !!! Done with Horizontal Enumeration**:checkered\_flag:&#x20;

#### Liked my work? Don't hesitate to buy me a coffee XDD

#### :heart::blue\_heart::green\_heart: [https://www.buymeacoffee.com/siddheshparab](https://www.buymeacoffee.com/siddheshparab) :green\_heart: :blue\_heart: :heart:&#x20;









