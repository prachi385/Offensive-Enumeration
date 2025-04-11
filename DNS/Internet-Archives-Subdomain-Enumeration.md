# 🌐 Internet Archives and Subdomain Enumeration

## 📖 Introduction

Internet Archives are web crawlers and indexing systems that periodically crawl websites, storing historical snapshots of their content. These archives can be valuable for discovering subdomains that once existed but may no longer be publicly visible. By analyzing past records, we can:

- 🔍 Identify old and forgotten subdomains  
- 🧪 Perform permutations to find more valid subdomains  
- 🛡️ Enhance reconnaissance efforts in security research and penetration testing  

---

## 🗂️ Extracting Historical Subdomains

### 📥 Querying the Internet Archive for URLs

We use tools like `waybackurls` or `gau` (GetAllUrls) to fetch historical URLs for a target domain.

#### Using `waybackurls`:
```bash
echo "example.com" | waybackurls > urls.txt
```

#### Using `gau`:
```bash
gau example.com > urls.txt
```

This will output a list of archived URLs related to `example.com`.

---

### 🧹 Extracting Unique Subdomains from URLs

Since we only need subdomains (not full URLs), we process them using `unfurl`, which extracts domain parts.

```bash
cat urls.txt | unfurl -u domains | sort -u > subdomains.txt
```

- `unfurl -u domains`: Extracts unique domains from the URL list  
- `sort -u`: Ensures we get only unique subdomains  

**Example output (`subdomains.txt`):**
```
blog.example.com  
mail.example.com  
dev.example.com
```

---

## 🧬 Generating Permutations of Subdomains

To discover more potential subdomains, we can use `dnsgen`:

```bash
cat subdomains.txt | dnsgen - > permutations.txt
```

**Example generated subdomains:**
```
admin.blog.example.com  
test.mail.example.com  
dev2.example.com
```

---

## 🔎 Validating and Resolving Subdomains

Once we have a list of potential subdomains, we need to verify  which ones are live.

### ✅ Using `dnsx` (DNS Resolution)

```bash
cat permutations.txt | dnsx -resp > valid_subdomains.txt
```

- `dnsx` resolves the domains and checks if they exist  
- `-resp`: Retrieves response data for further analysis

---

### 🌍 Using `httprobe` (Check HTTP/S Services)

```bash
cat valid_subdomains.txt | httprobe > live_subdomains.txt
```

- `httprobe` checks which subdomains have active web services

---

## ⚙️ Automating the Process

For efficiency, we can combine these steps into a single pipeline:

```bash
echo "example.com" | waybackurls | unfurl -u domains | sort -u | tee subdomains.txt
cat subdomains.txt | dnsgen | tee permutations.txt
cat permutations.txt | dnsx -resp | tee valid_subdomains.txt
cat valid_subdomains.txt | httprobe | tee live_subdomains.txt
```

This ensures a streamlined workflow from fetching historical data to identifying active subdomains.

---

## ✅ Conclusion

By leveraging Internet Archives and tools like:

- `waybackurls` or `gau`
- `unfurl`
- `dnsgen`
- `dnsx`
- `httprobe`

We can efficiently enumerate subdomains, even those that were once active but are no longer indexed by search engines.

---

### 👨‍💻 This approach is useful for:

- Security researchers conducting reconnaissance  
- Bug bounty hunters looking for overlooked attack surfaces  
- Organizations monitoring their digital footprint
