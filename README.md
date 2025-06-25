# Wakeup_bluetooth_fix

Intelbluetooth in Hackintosh is unstable. After wake up from S3 or S4 sleep, the bluetooth function might not work properly.

To solve this, we will make a script with sleepwatcher that performs "sudo pkill bluetoothd" when systems wakes up, installing Bluesnooze.

---

### **1. Acquire Sudo Privileges**

```bash
sudo -v
```

---

### **2. Uninstall Existing SleepWatcher (Optional)**

```bash
sudo launchctl unload /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist 
launchctl unload ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist
sudo rm -f /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
rm -f ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist
sudo rm -f /usr/local/sbin/sleepwatcher
sudo rm -f /usr/local/share/man/man8/sleepwatcher.8
```

---

### **3. Download and (Re)install SleepWatcher**

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

### **4. Enable and Configure SleepWatcher**


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

* rc.wakeup is not necessary
  
---

### **5. Modify rc.sleep script to pkill bluetoothd when system wakes up**

```bash
# Open /etc/rc.wakeup with nano
sudo nano /etc/rc.wakeup
```

Add the following line to your /etc/rc.sleep

```bash
#!/bin/sh

sudo pkill bluetoothd

exit 0
```

---

### **6. Modify sudoers to run sudo command in automated script**

```bash
# Open /etc/sudoers with nano
sudo nano /etc/sudoers
```

```bash
# Add the following line to the suoders
username ALL=(ALL) NOPASSWD: /etc/rc.wakeup pkill
```
* You may replace username into your account's name

---

### **7. Install Bluesnooze**

```bash
# Install Bluesnooze via Homebrew
brew install bluesnooze
```
* You may need to run the Bluesnooze app manually after its installation

---

### **8. Uninstall SleepWatcher (same as #2)**

```bash
sudo launchctl unload /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist 
launchctl unload ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist
sudo rm -f /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
rm -f ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist
sudo rm -f /usr/local/sbin/sleepwatcher
sudo rm -f /usr/local/share/man/man8/sleepwatcher.8
```
---
