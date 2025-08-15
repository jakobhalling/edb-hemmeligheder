Got it — here’s the complete single markdown file with everything we discussed, ready to drop into your repo as `README.md`:

---

```markdown
# GPU Crash Troubleshooting Setup (Windows + NVIDIA RTX)

This guide documents three complementary methods for capturing detailed crash data when troubleshooting NVIDIA GPU hangs, black screens, or driver resets on Windows.

---

## 1. NVIDIA Nsight Aftermath (GPU Crash Dumps)

**Purpose:**  
Captures low-level GPU crash dump files and metadata when the driver detects a fatal error. Useful for identifying driver-level hangs vs. hardware dropouts. Dumps can be sent to NVIDIA support or analyzed with developer tools.

**How to Enable:**
1. Download and install **Nsight Graphics** from NVIDIA:  
   https://developer.nvidia.com/nsight-graphics
2. During install, **only select `Nsight Aftermath SDK`** under *NVIDIA SDKs* for a minimal setup.
3. Run `NvAftermathMonitor.exe` (installed with the SDK).
4. Enable:
   - **Global crash dump collection** (or whitelist specific games).
   - **Generate GPU crash dumps**.
   - *(Optional)* Include shader debug info for more detail.
5. Choose an output folder for dumps.

**Sources:**  
- [NVIDIA Nsight Aftermath User Guide](https://docs.nvidia.com/nsight-aftermath/UserGuide/index.html)  
- [Nsight Graphics Download](https://developer.nvidia.com/nsight-graphics)

---

## 2. TDR (Timeout Detection & Recovery) Delay Adjustment

**Purpose:**  
Extends the time Windows waits before declaring the GPU unresponsive and resetting it. May allow recovery from brief driver hangs that would otherwise cause a crash.

**How to Configure:**
1. Open **Registry Editor** (`regedit`).
2. Navigate to:
```

HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Control\GraphicsDrivers

```
3. Add or modify these **DWORD (32-bit)** values:
- `TdrDelay` = `16` (decimal)
- `TdrDdiDelay` = `10` (decimal)
4. Restart Windows.

**Effect:**  
Windows will now allow up to **26 seconds total** (TdrDelay + TdrDdiDelay) before a GPU reset.

**Sources:**  
- [Microsoft TDR Registry Keys](https://learn.microsoft.com/en-us/windows-hardware/drivers/display/tdr-registry-keys)  
- [NVIDIA Support: TDR Delays](https://nvidia.custhelp.com/app/answers/detail/a_id/3335)

---

## 3. Windows Error Reporting (WER) LocalDumps

**Purpose:**  
Captures application crash dumps (full or mini) for any user-mode process, including games. Useful for analyzing application-level faults alongside GPU events.

**How to Configure (Global Dumps):**
1. Open **Registry Editor** (`regedit`).
2. Navigate to:
```

HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps

```
*(Create the `LocalDumps` key if it doesn’t exist.)*
3. Inside `LocalDumps`, add:
- `DumpFolder` → **Expandable String Value** (`REG_EXPAND_SZ`) → e.g. `D:\CrashDumps`
- `DumpCount` → **DWORD (32-bit)** → `16`
- `DumpType` → **DWORD (32-bit)** → `2` (full dump)
4. Create the dump folder if it doesn’t exist.
5. Restart Windows.

**Effect:**  
All application crashes will generate full dumps in the chosen folder until the dump count limit is reached.

**Sources:**  
- [Microsoft WER LocalDumps Documentation](https://learn.microsoft.com/en-us/windows/win32/wer/collecting-user-mode-dumps)  
- [Medal.tv Guide to Enabling Crash Dumps](https://support.medal.tv/support/solutions/articles/48001156648-how-to-enable-windows-crash-dumps)

---

## 4. Event Viewer Filter for GPU & Crash Events

**Purpose:**  
Quickly see only the relevant GPU and crash events that occur before each system crash.

**How to Set Up:**
1. Open **Event Viewer**.
2. Go to **Windows Logs → System**.
3. On the right-hand panel, click **Filter Current Log…**.
4. In **Event sources**, select:
- `Display`
- `nvlddmkm`
- `Kernel-Power`
- `Kernel-EventTracing`
- `EventLog`
5. In **Event IDs**, enter:
```

4101, 14, 1, 41

```
- `4101` — Display driver stopped responding.
- `14` — nvlddmkm device error.
- `1` — Kernel-EventTracing errors.
- `41` — Kernel-Power unexpected shutdown.

6. Click **OK** to apply the filter.

**Tip:**  
You can also create a **Custom View** in Event Viewer with these same settings so it’s always available.

**Sources:**  
- [Microsoft Event Viewer Filtering Guide](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-viewer-security-events)  
- [NVIDIA TDR Timeout Error 4101](https://nvidia.custhelp.com/app/answers/detail/a_id/5175)

---

## Recommended Usage
- **Use all three capture methods + Event Viewer filtering** for complete coverage:
- Nsight Aftermath: GPU driver/hardware-level hangs.
- TDR Delay: Chance to recover from borderline driver hangs.
- WER LocalDumps: Application-level crashes.
- Event Viewer Filter: Quickly correlate dumps with GPU/system logs.

- After reproducing the crash:
1. Check your WER dump folder (`D:\CrashDumps`) for `.dmp` files.
2. Check Nsight Aftermath dump folder for `.nv-gpudmp` files.
3. Review Event Viewer filtered logs to correlate timestamps and error codes.

---
```

---

Do you want me to also add **a ready-to-import Custom View XML** so you can just drop it into Event Viewer without manually setting filters? That would make it a one-click setup.
