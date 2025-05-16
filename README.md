# Wakeup_bluetooth_fix
Fixing bluetooth by turning off when the systems goes to sleep (S3,S4). It utilises **SleepWatcher**

macOS only

---

### 🛠️ **1. Acquire Sudo Privileges**

```bash
sudo -v
```

---

### 🚫 **2. Uninstall Existing SleepWatcher (Optional)**

```bash
sudo launchctl unload /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist 2>/dev/null
launchctl unload ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist 2>/dev/null
sudo rm -f /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
rm -f ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist
sudo rm -f /usr/local/sbin/sleepwatcher
sudo rm -f /usr/local/share/man/man8/sleepwatcher.8
```

---

### ⬇️ **3. Download and Reinstall SleepWatcher**

```bash
# download sleepwatcher package, untar, and cd into directory
curl --remote-name "http://www.bernhard-baehr.de/sleepwatcher_2.2.1.tgz"
tar xvzf sleepwatcher_2.2.1.tgz 2>/dev/null
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

* Creates placeholder files that SleepWatcher will look for and run when the system wakes up or goes to sleep.

### 🚫 **5. Turn-off Bluetooth when system goes sleep**

```bash
# Open /etc/rc.sleep with nano
sudo nano /etc/rc.sleep
```

```bash
# Add pkill bluetoothd in your /etc/rc.sleep
echo "YOUR_PASSWORD" | sudo -S pkill bluetoothd
```

* You must replace "Your_PASSWORD" into your Sudo password.

---
