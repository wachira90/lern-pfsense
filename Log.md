To clear logs and unused files in **pfSense version 2.7.2**, you can follow these safe and effective steps via the **Web GUI** or **SSH/Console**. Here's how:

---

## 🔧 **Web GUI Method**

### 1. **Clear System Logs via GUI**

* **Navigate to:** `Status` → `System Logs`
* Use the tabs (`System`, `Firewall`, `DHCP`, etc.) to view specific logs.
* Each tab has a **"Clear log"** or **trash bin icon** at the top. Click it to clear that log.

### 2. **Clear Package Logs**

* Navigate to `Diagnostics` → `Command Prompt`
* Run:

  ```sh
  rm -f /var/log/*pkg*
  ```

---

## 💻 **SSH or Console Method**

1. **Access SSH or pfSense console**

   * Optionally enable SSH: `System` → `Advanced` → `Admin Access` → Enable Secure Shell
   * Or use local console (e.g., through hypervisor or physical monitor)

2. **Clear All Logs**

   ```sh
   rm -f /var/log/*.log
   rm -f /var/log/*.gz
   ```

3. **Restart syslog (to recreate needed logs)**

   ```sh
   /etc/rc.restart_syslog
   ```

4. **Clear Suricata/Snort Logs (if installed)**

   ```sh
   rm -rf /var/log/suricata/*
   rm -rf /var/log/snort/*
   ```

5. **Remove old core dumps and crash reports**

   ```sh
   rm -f /var/crash/*
   ```

6. **Clear outdated packages’ leftover files**

   ```sh
   pkg clean -a
   ```

---

## 🧼 Optional: Clean Temporary and Cache Files

```sh
rm -rf /tmp/*
rm -rf /var/tmp/*
```

---

## 📦 Optional: Remove Unused Packages

If you're low on space, consider removing packages you don’t need:

* GUI: `System` → `Package Manager` → `Installed Packages` → Uninstall

---

## ✅ Best Practices

* **Backup before manual deletion**: `Diagnostics` → `Backup & Restore`
* **Automate log rotation** with log retention settings under `Status` → `System Logs` → `Settings`
* Monitor `/var` disk space with:

  ```sh
  df -h
  ```

Would you like a script to automate these steps?
