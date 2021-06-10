# Automation 🤖

It would be difficult for a person to perform all the above mentioned techniques. Hence, we need to rely on some kind of tool to automate such intensive steps. Wouldn't it be good if we just had to give our target name BOOM !!💥 the tool performs subdomain enumeration via all these techniques?

## [ReconFTW](https://github.com/six2dez/reconftw)

![](.gitbook/assets/reconftw_logo.png)

* Author: [six2dez](https://github.com/six2dez)
* Language: Bash

Yess this tool outperforms the work of subdomain enumeration via **6 unique techniques**. Currently if configured well, gives the most number of subdomains compared to any other open-source tool 🚀 . Let's take a look at the enumeration techniques it performs:-

1. **Passive Enumeration \(** [subfinder](https://github.com/projectdiscovery/subfinder), [assetfinder](https://github.com/tomnomnom/assetfinder), [amass](https://github.com/OWASP/Amass), [findomain](https://github.com/Findomain/Findomain), [crobat](https://github.com/cgboal/sonarsearch), [waybackurls](https://github.com/tomnomnom/waybackurls), [github-subdomains](https://github.com/gwen001/github-subdomains), [Anubis](https://jldc.me/), [gauplus](https://github.com/bp0lr/gauplus) and [mildew](https://github.com/daehee/mildew)\)
2. **Certificate transparency** \([ctfr](https://github.com/UnaPibaGeek/ctfr), [tls.bufferover](https://github.com/six2dez/reconftw/blob/main/tls.bufferover.run) and [dns.bufferover](https://github.com/six2dez/reconftw/blob/main/dns.bufferover.run)\)
3.  **Bruteforce** \([puredns](https://github.com/d3mondev/puredns)\)
4.  **Permutations** \([DNScewl](https://github.com/codingo/DNSCewl)\)
5. **JS files & Source Code Scraping** \([gospider](https://github.com/jaeles-project/gospider), [analyticsRelationship](https://github.com/Josue87/analyticsRelationship)\)
6. **DNS Records** \([dnsx](https://github.com/projectdiscovery/dnsx)\) 🤖 

### Installation:

* The [installer](https://github.com/six2dez/reconftw/blob/main/install.sh) script installs all the required dependencies and tools required.

```bash
git clone https://github.com/six2dez/reconftw
cd reconftw/
./install.sh
```

### Running ReconFTW:

* ReconFTW has a `-s` flag that performs subdomain enumeration & web probing.
* Out of all the 6 techniques if we want to skip any step we can do it through its [config](https://github.com/six2dez/reconftw/blob/main/reconftw.cfg#L49) file. Just set the value of a particular function to `false`
* Also, you can provide your own wordlist for bruteforcing by specifying them in the reconftw config file.
* Highly recommended that you run this tool in a VPS.

```bash
./reconftw.sh -d example.com -s
```

**Flags:**

* **d** - target domain
* **s** - Perform subdomain enumeration

{% hint style="success" %}
**Tip**: 🧙♂ Using `--deep` mode will run more time taking steps but return more subdomains
{% endhint %}

  


## Taking Subdomain Enumeration to next level 🚀 🚀 

The biggest fear while performing subdomain enumeration is that the public DNS resolvers we are using should give us a ban/timeout as we are querying them at a high rate for a prolonged period of time. Since we would be querying the public resolvers using our single VPS IP address they might give us a ban. But what we perform the same task by distributing the workload amongst several VPS instances? The chances of a ban would be less right? Also, the execution time would be considerably less right?

That's when **Axiom** comes to the rescue.

### [Axiom](https://github.com/pry0cc/axiom) 🤍

* Author: [pyrocc](https://github.com/pry0cc) 
* Language: Bash
* Supports: Digital Ocean, Linode, Azure, GCP, IBM

Axiom is a dynamic infrastructure that helps to distribute the workload of a single task equally among several cloud instances. A perfect tool while performing mass recon. You will first need to install Axiom on your VPS/system from where you will be able to spin up/down the cloud instances.

### How does axiom work?

 Let's consider want to perform DNS bruteforcing. For this first, you will need to initialize a fleet of instances. This can be any number of instances you want/authorize to make. Within a matter of 4-5 minutes that many instances would be initialized and ready to accept your commands.

#### Steps:

1. Divide the bruteforce wordlist into equal number\(total number of instances\) of parts 
2. Transfer each part to the respective instances
3. Perform standalone execution in separate instances
4. Merge the output results from all instances
5. Create a single output 

### Installation:

* Axiom has an interactive installer that will first ask for your cloud provider, API key, which provision to install, which region to choose, default instance size, etc.

```bash
git clone https://github.com/pry0cc/axiom ~/.axiom/
$HOME/.axiom/interact/axiom-configure
```

#### 

#### Some stats: 📊 \(30 instances\)

| Task | Axiom  | Single VPS |
| :--- | :--- | :--- |
| DNS bruteforcing \(11M wordlist\) | 8m 50s |  |
| Web probing \(\) |  |  |



![](.gitbook/assets/axiomxreconftw.png)

Yes, it's possible to integrate **Axiom** in **ReconFTW**. Isn't that great? 😍 

### Usage:

* It's necessary to first install ReconFTW first on your controller/main system and then install/setup axiom.
* Before running ReconFTW over Axiom it's recommended that you first initialize your fleet.
* The thing to note here is to run ReconFTW over Axiom you have to use another script called `reconftw_axiom.sh` 

```bash
axiom-fleet testy -i=30
axiom-select 'testy*'
./reconftw_axiom.sh -d example.com -s
```



