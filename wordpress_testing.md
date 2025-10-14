# How to test a WordPress site (and its plugins) 
## Quick overview — workflow

1. **Get permission & backup** (always).
2. **Recon & footprinting** — discover site info and plugins/themes.
3. **Automated scanning** — non-destructive: version/plugin checks, security scan.
4. **Manual verification** — confirm important findings, check config, headers, file permissions.
5. **Credentialed checks** — log in with a test admin account to check deeper issues.
6. **Triage & report** — prioritize fixable items (exploitable, public-facing, high-impact).
7. **Remediate + harden** — updates, configuration, WAF, monitoring.
8. **Re-test** to verify fixes.

---

## Tools you’ll want (short)

* **WPScan** (best-in-class WP vulnerability scanner / plugin DB)
* **Nmap** (port/service view, optional)
* **Nikto** (webserver checks)
* **Burp Suite** (manual web testing / intercept)
* **curl**, **wget**, **openssl s_client** (banner & headers)
* **WP-CLI** (for local/SSH credentialed checks and updates)
* **Searchsploit**, **Exploit DB** (for researching disclosed exploits — read-only)
* **Online checks**: Sucuri sitecheck, securityheaders.com, observatory.mozilla.org
* **Vulnerability databases**: WPScan Vulnerability DB, NVD/CVE feeds

---

## 0) Preparations (must-do)

* Obtain **written authorization** and scope (domains, IPs, test window).
* Take a **backup** (DB + files) or ask the owner to take one before scanning.
* Prefer non-destructive scans during the engagement (avoid high-load checks on production).

---

## 1) Passive recon / footprinting

* Check robots, sitemap, and `wp-json` to enumerate.

  * `https://example.com/wp-json/` — may reveal theme & plugin info.
* Look for common disclosure files:

  * `https://example.com/readme.html` (reveals WP version)
  * `https://example.com/wp-login.php` (login exists)
  * `https://example.com/wp-includes/` and `https://example.com/wp-content/plugins/`
* HTTP headers & TLS:

  * `curl -I https://example.com`
  * `openssl s_client -connect example.com:443 -servername example.com | openssl x509 -noout -text`

---

## 2) Plugin & theme enumeration

Common ways plugins leak:

* `https://example.com/wp-content/plugins/` (directory listing might be disabled; specific plugin folders can be probed)
* `https://example.com/wp-content/plugins/<plugin-folder>/readme.txt` or `readme.html`
* `https://example.com/wp-content/themes/<theme>/style.css` contains theme info
* `wp-json/wp/v2/plugins` — some sites expose plugin info via REST (rare)

**Manual checks**

* Try to fetch likely plugin paths:

  * `curl -s -I https://example.com/wp-content/plugins/akismet/`
* Use `wget` or a small script to test known plugin file paths safely (do not brute-force).

---

## 3) Automated scanning (safe, non-destructive)

### WPScan (recommended)

* WPScan uses its own WP vulnerability DB and is tailored to WordPress.
* Example (replace `YOUR_TOKEN` with a WPScan API token for more detailed plugin vulns):

```bash
# Unauthenticated scan (safe)
wpscan --url https://example.com --enumerate vp,vt,tt,u

# With API token for up-to-date vulnerability checks
wpscan --url https://example.com --api-token YOUR_WPSCAN_API_TOKEN --enumerate u,p,t,tt,cb,dbe --update
```

Key `--enumerate` switches:

* `p` = plugins
* `vp` = vulnerable plugins
* `t` = themes
* `u` = users
* `tt` = timthumb files
* `cb` = config backups

Interpretation: WPScan will list plugin names, versions (if detectable), and known CVEs or vulnerabilities from its DB.

**Notes:** WPScan can be run aggressively, but default enumerations are usually non-destructive. Still, get permission.

### Other scanners

* **Nikto**: webserver-level checks (CGIs, headers) — may be noisy.
* **Nessus/OpenVAS**: broader vulnerability scanning (use in lab or with permission).

---

## 4) Manual verification & targeted checks (do NOT exploit)

Once scanner flags a plugin or theme as vulnerable, **do manual verification**:

* Confirm version numbers via plugin files (if accessible) or DB (credentialed).
* Check CVE details and proof-of-concept availability on public DBs (read-only).
* For suspected SQLi, LFI, RCE — **do not** run an exploit on production. Instead:

  * Reproduce safely in a staging environment.
  * Look for relevant code paths (plugins with file upload, eval, insecure includes).

Common manual techniques:

* **Banner/header checks** with `curl -I` to confirm server version.
* **Check for open file uploads** (e.g., unprotected upload endpoints).
* **Check AJAX endpoints** (`admin-ajax.php`) for missing capability checks.
* **Search for known vulnerable files** in plugin folders (e.g., outdated timthumb).

---

## 5) Credentialed checks (higher confidence)

If you have a test admin account (recommended for deeper checks):

* Use **WP-CLI** over SSH if you have server access:

  * `wp plugin list --path=/var/www/html` — shows installed plugins and versions.
  * `wp core check-update`, `wp plugin update --dry-run`
* Use WPScan with credentials:

  ```bash
  wpscan --url https://example.com --api-token TOKEN --username admin --password 'P@ssw0rd' --enumerate p,vp
  ```
* Log in and inspect **installed plugins**, check their version pages, and test file permissions (e.g., can plugin files be edited?).

Credentialed scans reveal missing patches and configuration weaknesses not visible externally.

---

## 6) Common vulnerability categories to look for in plugins

* **Arbitrary file upload** → remote code upload (high risk).
* **SQL Injection / XSS** (reflected/stored) in plugin pages or forms.
* **Unauthenticated access / privilege escalation** (author pages exposing admin functions).
* **Local File Inclusion (LFI) / Remote File Inclusion (RFI)** via insecure include paths.
* **Cross-Site Request Forgery (CSRF)** on admin actions.
* **Insecure direct object references (IDOR)** for media or settings.
* **Outdated libraries** bundled with the plugin (e.g., old PHPMailer).

---

## 7) Hardening & remediation checklist (priority)

1. **Update core, plugins, and themes** to latest stable versions (first action).
2. **Remove unused plugins & themes** (attack surface reduction).
3. **Harden wp-config.php**:

   * Move it above webroot if possible.
   * Ensure secure DB credentials and unique salts (`WP_SALT`).
   * Disable file editing: `define('DISALLOW_FILE_EDIT', true);`
4. **Enforce strong file permissions** (typical: files 644, dirs 755; wp-config.php 600 or 640).
5. **Use a WAF** (Cloudflare, Sucuri, ModSecurity) for blocking known exploit patterns.
6. **Limit access to wp-admin & XML-RPC** (IP allow, rate-limiting, or disable XML-RPC if not needed).
7. **Use two-factor authentication** for admin accounts.
8. **Regular backups & patching schedule**; test restore.
9. **Implement logging & monitoring** (alerts for file changes, new admin users).
10. **Least privilege DB user** (no unnecessary permissions).

---

## 8) Reporting & triage (how to prioritize)

* **Critical**: authenticated RCE, plugin with public exploit and exposed endpoint.
* **High**: unauthenticated file upload, SQLi leading to data leak, admin brute-force possible.
* **Medium**: information disclosure (versions), TLS weak cipher.
* **Low**: security headers missing (recommend but lower urgency).

Always include:

* Evidence (screenshots, WPScan output snippets), steps to reproduce (non-destructive), and remediation guidance.

---

## 9) Safe practice / red flags

* Don’t run destructive or exploitative checks on production without explicit permission and backups.
* Beware of scanners that attempt many payloads — they can break poorly coded sites.
* If you find a critical issue, coordinate disclosure and fixes; for real clients follow their incident handling policy.

---

## 10) Example quick checklist you can hand students

* [ ] Get written permission + backup.
* [ ] Run `wpscan --url <site> --enumerate vp,vt,p,t --api-token <token>`
* [ ] Check `readme.html`, `wp-json`, plugin folders for leaks.
* [ ] Manual headers: `curl -I`, `openssl s_client`.
* [ ] Credentialed `wp plugin list` with WP-CLI (if possible).
* [ ] Check file perms and `wp-config.php` security.
* [ ] Remove unused plugins & update everything.
* [ ] Add WAF + 2FA + disable file editor.
* [ ] Produce prioritized remediation report.

---

If you want, I can:

* Generate a **step-by-step lab handout** that students can follow (with exact commands and expected output) — ideal for a 90-minute lab.
* Produce a **sample vulnerability report template** for findings from WPScan and manual checks.
* Create a **safe demo staging environment (Vagrant / Docker)** recipe for students to practise without touching production.

Which of those should I make next?
