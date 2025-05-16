# Wakeup_bluetooth_fix
Fixing bluetooth by turning off when the systems goes to sleep (S3,S4) with **SleepWatcher**

This script will execute "sudo pkill bluetoothd" when the system goes to sleep (S3, S4)

---

### 🛠️ **1. Acquire Sudo Privileges**

```bash
sudo -v
```

---

### 🚫 **2. Uninstall Existing SleepWatcher (Optional)**

```bash
sudo launchctl unload /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist 
launchctl unload ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist
sudo rm -f /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
rm -f ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist
sudo rm -f /usr/local/sbin/sleepwatcher
sudo rm -f /usr/local/share/man/man8/sleepwatcher.8
```

---

### ⬇️ **3. Download and (Re)install SleepWatcher**

```bash
# download sleepwatcher package, untar, and cd into directory
curl --remote-name "https://www.bernhard-baehr.de/sleepwatcher_2.2.1.tgz"
tar xvzf sleepwatcher_2.2.1.tgz
cd sleepwatcher_2.2.1
```

* Downloads and extracts the SleepWatcher version 2.2.1 package, you may change the version of Sleepwatcher.

```bash
# create folders necessary for installation
sudo mkdir -p /usr/local/sbin /usr/local/share/man/man8
```

```bash
# move files into installation folders
sudo cp sleepwatcher /usr/local/sbin
sudo cp sleepwatcher.8 /usr/local/share/man/man8
sudo cp config/de.bernhard-baehr.sleepwatcher-20compatibility.plist /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
```

---

### ✅ **4. Enable and Configure SleepWatcher**


```bash
# load launch agent
sudo launchctl load -w -F /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
```


```bash
# create script in local user directory and make them executable
sudo touch /etc/rc.wakeup
sudo touch /etc/rc.sleep
sudo chmod +x /etc/rc.sleep /etc/rc.wakeup
```

* It creates placeholder files (rc.wakeup, rc.sleep) that SleepWatcher will look for and run when the system wakes up or goes to sleep.

---

### 🚫 **5. Turn-off Bluetooth when system goes sleep**

```bash
# Open /etc/rc.sleep with nano
sudo nano /etc/rc.sleep
```

```bash
# Add the following line to your /etc/rc.sleep
echo "YOUR_PASSWORD" | sudo -S pkill bluetoothd
```

* You must replace "Your_PASSWORD" into your Sudo password.

---

### **6. Cleanup**

```bash
rm ~/sleepwatcher_2.2.1.tgz
rm -r ~/sleepwatcher_2.2.1
```

---
