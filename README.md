# Wakeup_bluetooth_fix
Fixing bluetooth by turning off when the systems goes to sleep (S3,S4). It utilises **SleepWatcher**

---

### 🛠️ **1. Acquire Sudo Privileges**

```bash
sudo -v
```


### 🚫 **2. Uninstall Existing SleepWatcher (Cleanup)**

```bash
# unload launch agents
sudo launchctl unload /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist 2>/dev/null
launchctl unload ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist 2>/dev/null
```

* Stops existing SleepWatcher daemons (both system-wide and user-specific).

```bash
# remove plist launchagents
sudo rm -f /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
rm -f ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist
```

* Deletes the configuration files (`plist`) that define how and when the daemon runs.

```bash
# remove executable and man files
sudo rm -f /usr/local/sbin/sleepwatcher
sudo rm -f /usr/local/share/man/man8/sleepwatcher.8
```

* Deletes the actual SleepWatcher binary and its manual entry from your system.

---

### ⬇️ **Download and Reinstall SleepWatcher**

```bash
# download sleepwatcher package, untar, and cd into directory
curl --remote-name "http://www.bernhard-baehr.de/sleepwatcher_2.2.tgz"
tar xvzf sleepwatcher_2.2.tgz 2>/dev/null
cd sleepwatcher_2.2
```

* Downloads and extracts the SleepWatcher version 2.2 package.

```bash
# create folders necessary for installation
sudo mkdir -p /usr/local/sbin /usr/local/share/man/man8
```

* Ensures the directories for the binary and manual exist.

```bash
# move files into installation folders
sudo cp sleepwatcher /usr/local/sbin
sudo cp sleepwatcher.8 /usr/local/share/man/man8
sudo cp config/de.bernhard-baehr.sleepwatcher-20compatibility.plist /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
```

* Installs the binary, man page, and launch daemon configuration.

---

### ✅ **Enable and Configure SleepWatcher**

```bash
sleep 1
```

* Small delay to let file operations complete.

```bash
# load launch agent
sudo launchctl load -w -F /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
```

* Loads the SleepWatcher daemon so it starts running on system boot.

```bash
# create script in local user directory and make them executable
sudo touch /etc/rc.wakeup
sudo touch /etc/rc.sleep
sudo chmod +x /etc/rc.sleep /etc/rc.wakeup
```

* Creates placeholder files that SleepWatcher will look for and run when the system wakes up or goes to sleep.
* Marks them as executable.

