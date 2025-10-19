## 💾 Phase 2: TimeMachine-Style Backup System
> **Goal:** Configure automatic, BTRFS-based snapshots with Timeshift.

### 2.1 Configure Timeshift
1. Open **Timeshift** from the Applications menu.  
2. Select **BTRFS** snapshot type.  
3. Configure snapshot schedule – Boot 3, Daily 5, Weekly 3, Monthly 2.  
4. Enable “Include Home directories.”  
5. Finish setup.  

> 💾 Timeshift will now automatically create and manage snapshots.

### 2.2 Optional: External Backup Drive
> 🔐 Connect your external drive and select it under *Settings → Backup Device* in Timeshift.

### 2.3 Test Your Backup Setup
```bash
sudo timeshift --create --comments "Initial setup test"
sudo timeshift --list
```
> 💡 You can also test manually via GUI → **Create Snapshot**.

### 2.4 Verify Automatic Backups
Open Timeshift → Settings → Schedule and confirm daily/weekly/monthly snapshot routines.

---

