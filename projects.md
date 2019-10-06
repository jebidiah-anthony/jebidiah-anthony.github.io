---
title: What Not
layout: default
---

# PROJECTS

## >>[Setting up a Windows Event Collector](./prjct/Windows-Event-Collector.html)<<
> This lays out how to create a sbuscription (both source and collector initiated) that collects selected forwarded __`.evtx`__  event logs from a workstation to a domain controller.

## >>[Creating Custom __*.evtx*__ Logfiles](./prjct/Custom-evtx-Logfiles.html)<<
> This shows the process of how to create custom __`.evtx`__ log files using __`ecmangen.exe`__ and other utilities present in the [__Windows Development Kit__](https://go.microsoft.com/fwlink/p/?LinkId=838916). The log file(s) created could be used as a destination log for forwarded events.

## >>[Pass-the-Ticket: PSRemoting Double-hop Bypass](./prjct/PTT-PSRemoting.html)<<
> The double-hop problem occurs when, for example, a local PowerShell instance connected via PSRemoting to a remote server which is connected to the target server and an attempt to execute commands on the target server was made and was rejected. The end goal of this proof-of-concept is to execute a pass-the-ticket attack on an active directory while being remotely connected to a domain computer with administrator privileges.