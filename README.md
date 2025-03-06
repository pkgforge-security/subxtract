### ℹ️ About
Wrapper around [go-fasttld](https://github.com/elliotwutingfeng/go-fasttld) for extracting **`TLD`** (Top-Level-Domains) & **`ROOT_DOMAIN`** using ***accurate*** and ***always-uptodate*** `public-suffix` list from **domains**, **urls**, **ipv4**, **ipv6**, etc.<br>
Tools like [tomnomnom/unfurl](https://github.com/tomnomnom/unfurl) rely on [Hardcoded Regexes](https://github.com/tomnomnom/unfurl/blob/master/main.go) and thus fail to properly separate tld-suffix from actual domains.<br>
Instead, [go-fasttld](https://github.com/elliotwutingfeng/go-fasttld) uses [Public Suffix List](https://raw.githubusercontent.com/publicsuffix/list/master/public_suffix_list.dat) which is autoupdated regularly.

### 🖳 Installation
Use [soar](https://github.com/pkgforge/soar) & Run:
```bash
soar add 'subxtract#github.com.pkgforge-security.subxtract'
```

### 🧰 Usage
```mathematica
❯ subxtract --help
➼ Usage: subxtract -f </path/to/domain/urls.txt> <opts>
        : subxtract "https://a.b.c.example.com.np" <opts>
STDIN   : cat </path/to/domain/urls.txt> | subxtract <opts>
          echo "https://a.b.c.example.com.np" | subxtract <opts>
Flags:
  -c, --concurrency int     Limit the number of concurrent goroutines (default 50)
  -d, --domains             Print the root domain and suffix combined
  -f, --file string         Input file containing URLs|Domains (one per line)
  -h, --help                help for subxtract
  -i, --ignore-subdomains   Ignore (Exclude) subdomains
  -j, --json                Output in JSON Format (Everything)
  -s, --private-suffix      Include Private Suffix (Example: blogspot.com)
  -p, --punycode            Convert Internationalized Domain Names (IDN) to Punycode (ASCII Characters)
  -r, --roots               Print only the root domain (without suffix)
  -t, --tlds                Print only the Top Level Domain (TLD)
```

---
### Examples:
- #### Input
**`cat domains.txt`** 
> - You can also pass `urls`, doesn't really matter
> - **Don't pass wildcards `(.*)`**
```bash
abc.example.com
abc.example.net
apple.com
be.banana.com
example.com
example.com.np
example.net
example.org
xyz.abc.example.com
xyz.abc.example.net
```
---
- To Extract **`Root Domain Names`** ( **NO `.tld`**)
```bash
subxtract -f domains.txt -r | awk '!seen[$0]++'
!# Or Via STDIN
cat domains.txt | subxtract -r | awk '!seen[$0]++'
```
- #### Output
```bash
example
apple
banana
```
---
- Similarly, To Extract **`Root Domain Names`** **`+`** **`Top Level Domains (TLDs)`**
```bash
subxtract -f domains.txt -d | awk '!seen[$0]++'
!# Or Via STDIN
cat domains.txt | subxtract -d | awk '!seen[$0]++'
```
- #### Output
```bash
example.com
apple.com
banana.com
example.net
example.org
example.com.np
```