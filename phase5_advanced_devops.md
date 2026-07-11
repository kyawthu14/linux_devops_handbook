# Phase 5: Advanced DevOps & SRE Skills

This file contains the commands, explanations, warnings, and verification questions we covered during Phase 5.

---

## 5.1 Task Automation & Scheduling (`cron`)

Task automation is essential for running backups, rotations, and maintenance scripts.

### Cron Time Fields (The 5 stars)
```text
* * * * *  command_to_execute
- - - - -
| | | | |
| | | | +----- day of week (0 - 6) (Sunday is 0)
| | | +------- month (1 - 12)
| | +--------- day of month (1 - 31)
| +----------- hour (0 - 23)
+------------- minute (0 - 59)
```

### Key Cron Commands
* **`crontab -e`:** Opens your user's crontab schedule list in a text editor.
* **`crontab -l`:** Lists all active scheduled cron jobs.
* **`crontab -r`:** Deletes your entire crontab (⚠️ **WARNING:** Deletes without warning, use carefully!).
* **DevOps Shortcut (Pipe configuration):**
  `echo "*/1 * * * * /home/kt/script.sh" | crontab -` (pipes the rule directly into the cron list without launching an interactive editor).

### Examples
* `*/15 * * * * /path/to/script.sh` -> Runs every 15 minutes.
* `0 2 * * 0 /path/to/script.sh` -> Runs at exactly 2:00 AM every Sunday.

---

## 5.2 Log Analysis & Troubleshooting

### Log Locations
* **`/var/log/syslog`:** Main system log containing general OS and service logs.
* **`/var/log/auth.log`:** Security logs capturing sudo execution and logins.
* **`/var/log/apt/`:** Location of installation histories.

### Parsing Logs
* `sudo tail -f /var/log/syslog`
* `sudo grep "COMMAND=" /var/log/auth.log` (audit trail of executed sudo commands).

### Log Rotation (`logrotate`)
Automatically manages growing log files to prevent filling the disk.
* Configs located in `/etc/logrotate.d/` (e.g. `/etc/logrotate.d/apt`).
* Key parameters:
  * `rotate 12` (keep 12 backup archives).
  * `monthly` / `weekly` (how often to rotate).
  * `compress` (compress rotated logs into `.gz` using gzip to save 90% space).
  * `notifempty` (do not rotate empty log files).

---

## Verification Questions We Covered

1. **Q:** Translate this cron rule: `30 1 * * 0 /home/kt/cleanup.sh`.
   * **A:** Run at 1:30 AM every Sunday.
2. **Q:** How do you write a cron rule to run a script `/home/kt/status_check.sh` every 15 minutes?
   * **A:** `*/15 * * * * /home/kt/status_check.sh`
3. **Q:** How do you list the active cron jobs?
   * **A:** `crontab -l`
4. **Q:** Which log file contains security information like failed SSH attempts and sudo logs?
   * **A:** `/var/log/auth.log`
5. **Q:** Why is the `logrotate` utility critical, and what does the `compress` option do?
   * **A:** It prevents log files from consuming all disk space and crashing the server. The `compress` option gzip-compresses old logs to save space.
6. **Q:** How do you search `/var/log/syslog` for "error" case-insensitively?
   * **A:** `sudo grep -i "error" /var/log/syslog`
