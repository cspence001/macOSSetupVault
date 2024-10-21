Here’s a reformatted version of the notes for clarity and organization:

---

### **Process Logging**

#### **Outputting Log Info from `launchd.log`**
```bash
% grep "somePattern" /var/log/launchd.log > output.txt
```

#### **Using `log show`**

- **View Recent `error` Logs**:
  ```bash
  log show --predicate 'eventMessage contains "error"' --info
  ```

- **Check System Logs**:
  ```bash
  log show --style syslog
  ```

#### **Process Log with Date Selection**

- **For `configd` Process**:
  ```bash
  % log show --predicate 'process == "configd"' --info --start "2024-08-02 00:00:00" --end "2024-08-02 23:59:59" > configdlog.txt
  ```

- **For `Brave Browser` Process**:
  ```bash
  % log show --predicate 'process == "Brave Browser"' --info --start "2024-08-02 00:00:00" --end "2024-08-02 23:59:59" > 0802Bravelog.txt
  ```

- **For `mDNSResponder` Sender**:
  ```bash
  % sudo log show --style syslog --predicate 'senderImagePath contains[cd] "mDNSResponder"' --info --start "2024-08-06 00:00:00" --end "2024-08-06 23:59:59"
  ```

#### **Using `log stats`**

- **Options**:
  - **`--archive <archive>`**: Specify a log archive to display statistics from.
  - **`--count <count>` | `all`**: Limit output to a specific number of rows per section or use `all` to display all rows.
  - **`--sort events | bytes`**: Sort output by the number of events or the number of bytes.
  - **`--style human | json`**: Choose the output style: human-readable or JSON.
  - **`--overview`**: Default mode to display statistics for the entire log archive.
  - **`--per-book`**: Display statistics per log book.
  - **`--per-file`**: Display statistics per file.
  - **`--sender <sender>`**: Show statistics for a specific sender.
  - **`--process <process>`**: Show statistics for a specific process.
  - **`--predicate <predicate>`**: Display statistics for events matching a given predicate.

- **Examples**:
  1. **Display Overview Statistics**:
     ```bash
     log stats --overview
     ```

  2. **Display Statistics with a Specific Count**:
     ```bash
     log stats --count 10
     ```

  3. **Sort Output by Events**:
     ```bash
     log stats --sort events --overview
     ```

  4. **Display Statistics in JSON Format**:
     ```bash
     log stats --style json --overview
     ```

  5. **Show Statistics for a Specific Process**:
     ```bash
     log stats --process <process_name>
     ```

  6. **Display Statistics for Events Matching a Predicate**:
     ```bash
     log stats --predicate 'eventMessage contains "error"'
     ```

---
