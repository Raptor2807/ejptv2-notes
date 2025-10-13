---
tags: [ejptv2, study-plan, nmap, metasploit, pentesting]
created: {{date}}
deadline: 2025-11-30
study_days_per_week: 4
avg_daily_hours: 5
total_hours: 139.5
---

# eJPTv2 Study Plan — Finish by 30 Nov 2025

Goal: Complete all remaining sections before **30 November 2025** by studying **4 days/week (~5h/day)**.  
Total ≈ **139h 33m**, **28 study days**, **7 weeks**.

---

## 📅 Week 1 (Oct 13–19)
- [ ] **Oct 13 (Mon)** — Enumeration — 4h59m  
- [ ] **Oct 14 (Tue)** — Enumeration 3h46m + Vulnerability Assessment 1h13m — 4h59m  
- [ ] **Oct 16 (Thu)** — Vulnerability Assessment — 4h59m  
- [ ] **Oct 18 (Sat)** — Vulnerability Assessment 0h48m + Auditing Fundamentals 3h58m + Host/System Attacks 0h13m — 4h59m  

---

## 📅 Week 2 (Oct 20–26)
- [ ] **Oct 20 (Mon)** — Host/System Attacks — 4h59m  
- [ ] **Oct 21 (Tue)** — Host/System Attacks — 4h59m  
- [ ] **Oct 23 (Thu)** — Host/System Attacks — 4h59m  
- [ ] **Oct 25 (Sat)** — Host/System Attacks — 4h59m  

---

## 📅 Week 3 (Oct 27–Nov 2)
- [ ] **Oct 27 (Mon)** — Host/System Attacks 0h20m + Network Attacks 4h39m — 4h59m  
- [ ] **Oct 28 (Tue)** — Network Attacks 0h08m + Metasploit Framework 4h51m — 4h59m  
- [ ] **Oct 30 (Thu)** — Metasploit Framework — 4h59m  
- [ ] **Nov 1 (Sat)** — Metasploit Framework — 4h59m  

---

## 📅 Week 4 (Nov 3–9)
- [ ] **Nov 3 (Mon)** — Metasploit Framework — 4h59m  
- [ ] **Nov 4 (Tue)** — Metasploit Framework — 4h59m  
- [ ] **Nov 6 (Thu)** — Metasploit Framework — 4h59m  
- [ ] **Nov 8 (Sat)** — Metasploit Framework — 4h59m  

---

## 📅 Week 5 (Nov 10–16)
- [ ] **Nov 10 (Mon)** — Metasploit 0h43m + Exploitation 4h16m — 4h59m  
- [ ] **Nov 11 (Tue)** — Exploitation — 4h59m  
- [ ] **Nov 13 (Thu)** — Exploitation — 4h59m  
- [ ] **Nov 15 (Sat)** — Exploitation — 4h59m  

---

## 📅 Week 6 (Nov 17–23)
- [ ] **Nov 17 (Mon)** — Exploitation 1h36m + Post-Exploitation 3h23m — 4h59m  
- [ ] **Nov 18 (Tue)** — Post-Exploitation — 4h59m  
- [ ] **Nov 20 (Thu)** — Post-Exploitation — 4h59m  
- [ ] **Nov 22 (Sat)** — Post-Exploitation — 4h59m  

---

## 📅 Week 7 (Nov 24–30)
- [ ] **Nov 24 (Mon)** — Post-Exploitation — 4h59m  
- [ ] **Nov 25 (Tue)** — Post-Exploitation 3h14m + Social Engineering 1h45m — 4h59m  
- [ ] **Nov 27 (Thu)** — Social Engineering 1h26m + Web App Pentesting 3h33m — 4h59m  
- [ ] **Nov 29 (Sat)** — Web App Pentesting — 4h59m  

---

## 🧭 Notes
- Allocate breaks every 90 minutes.  
- Keep daily notes inside `/eJPTv2 Journal/` folder for review.  
- Focus on **Metasploit**, **Post-Exploitation**, and **Exploitation** labs — most exam weight.  
- Add a “✅ Completed” tag per finished week.

---

## 🏁 Completion Checklist
- [ ] Enumeration  
- [ ] Vulnerability Assessment  
- [ ] Auditing Fundamentals  
- [ ] Host & System Attacks  
- [ ] Network Based Attacks  
- [ ] Metasploit Framework  
- [ ] Exploitation  
- [ ] Post-Exploitation  
- [ ] Social Engineering  
- [ ] Web App Pen Testing  

---

Progress can be visualized using the Obsidian Dataview plugin if desired.

```markdown
TABLE file.link as "Week", sum(contains(text, "✅")) as "Completed Days"
FROM "ejptv2"
WHERE contains(file.tags, "study-plan")
