# Minimal Windows Service Template

for demonstrating privilege escalation via weak service executable permissions.

If a service binary file is writable by a low-priv user
(check ACLs with [icacls](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/icacls) Windows command or
[AccessChk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) from [Sysinternals](https://docs.microsoft.com/en-us/sysinternals/)),
and the service binary runs with high-priv rights (e.g. `LocalSystem`,
check it with `sc qc <service name>`), replacing the binary
with your malicious service binary should elevate to system rights.

Here is a sample service source template with customizable payload:
[service.c](./service.c).

The example payload adds a custom admin user to the system.
Payload is implemented using Windows API calls (instead of calling
external `net` commands) in order to be much more silent,
meaning easier AV bypass.

Cross-compiling works using [MinGW](http://www.mingw.org/) on Linux:

```
x86_64-w64-mingw32-gcc service.c -lnetapi32 -s -o service.exe
```

Compiling was tested on [Arch Linux](https://www.archlinux.org/)
with [BlackArch](https://blackarch.org/) repos (for MinGW),
Service was tested on Windows 10 Pro (Build 18363).

