# ms11-046-mingw

CVE-2011-1249 / MS11-046 — AFD.sys privilege escalation for Windows x86.

Original exploit by Tomislav Paskalev (EDB-40564). This fork fixes MinGW
cross-compilation errors and adds custom command execution via argv[1].

---

## Changes from the original

| # | Change | Why |
|---|--------|-----|
| 1 | `FARPROC ZwQuerySystemInformation` → typed function pointer (`pZwQuerySystemInformation`) | MinGW strict type checking rejects calling a zero-parameter `FARPROC` with arguments — compilation fails with *too many arguments to function* |
| 2 | `securityPatches[]` arrays marked `static` | Arrays declared inside `if/else` blocks are stack-allocated and go out of scope; without `static` the pointers dangle |
| 3 | Custom command via `argv[1]` | Run any command as SYSTEM instead of the hardcoded `cmd.exe` |

---

## Affected versions

Windows x86 (32-bit):
- Windows XP SP3
- Windows Vista SP1 / SP2
- Windows Server 2003 SP2
- Windows Server 2008 SP2
- Windows 7

Not affected: x64, Windows 7 SP1+, Server 2008 R2+.

---

## Cross-compile on Linux (MinGW)

```bash
sudo apt install mingw-w64
i686-w64-mingw32-gcc ms11-046.c -o ms11-046.exe -lws2_32
