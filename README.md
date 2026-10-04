# Homelab

Sprache wählen / Choose a language

<details open name="language">
<summary><b>Deutsch</b></summary>

Ich betreibe virtuelle Maschinen mit Ubuntu Server und CentOS in Oracle VirtualBox und verwalte sie von Windows PowerShell aus per SSH. In diesem Homelab übe ich Linux-Administration, Netzwerktechnik und Fernwartung.

## Architekturdiagramm

```mermaid
%%{init: {"themeVariables": {"fontSize": "24px", "edgeLabelBackground": "#ffffff", "textColor": "#000000"}, "flowchart": {"nodeSpacing": 80, "rankSpacing": 80, "padding": 24, "wrappingWidth": 500}}}%%
flowchart TB
    PS["<b>⌨️ Windows PowerShell<br/>SSH-Client</b>"]

    subgraph HOST["<b>📦 Oracle VM VirtualBox (Host)</b>"]
        NET{{"<b>🌐 Virtuelles Netzwerk<br/>VMs erreichen sich per Ping / SSH</b>"}}
        UB["<b>🟠 Ubuntu Server<br/>Gast-VM</b>"]
        CE["<b>🔵 CentOS<br/>Gast-VM</b>"]
    end

    PS -->|"<b>SSH :22</b>"| NET
    NET --> UB
    NET --> CE

    classDef client fill:#dbeafe,stroke:#1e3a8a,stroke-width:2px,color:#000
    classDef net fill:#e5e7eb,stroke:#374151,stroke-width:2px,color:#000
    classDef ubuntu fill:#fed7aa,stroke:#9a3412,stroke-width:2px,color:#000
    classDef centos fill:#c7d2fe,stroke:#3730a3,stroke-width:2px,color:#000
    class PS client
    class NET net
    class UB ubuntu
    class CE centos
    style HOST fill:#fef9c3,stroke:#854d0e,stroke-width:2px,color:#000
```

## Wie alles verbunden ist

```mermaid
%%{init: {"themeVariables": {"fontSize": "22px"}, "flowchart": {"nodeSpacing": 40, "rankSpacing": 50, "padding": 20, "wrappingWidth": 400}}}%%
flowchart TB
    subgraph UBU["<b>🟠 Schritt 1 - Verbindung zu Ubuntu</b>"]
        direction LR
        U1["<b>⌨️ PowerShell<br/>SSH zu Ubuntu</b>"] --> U2["<b>🌐 VM-Netzwerk<br/>Port 22</b>"] --> U3["<b>🟠 Ubuntu Server<br/>Shell-Sitzung startet</b>"]
    end

    subgraph CEN["<b>🔵 Schritt 2 - Verbindung zu CentOS</b>"]
        direction LR
        C1["<b>⌨️ PowerShell<br/>SSH zu CentOS</b>"] --> C2["<b>🌐 VM-Netzwerk<br/>Port 22</b>"] --> C3["<b>🔵 CentOS<br/>Shell-Sitzung startet</b>"]
    end

    subgraph VMS["<b>🔁 Schritt 3 - VM zu VM</b>"]
        direction LR
        V1["<b>🟠 Ubuntu Server<br/>Ping / SSH</b>"] <--> V2["<b>🔵 CentOS<br/>Antwort</b>"]
    end

    UBU ~~~ CEN ~~~ VMS

    classDef pc fill:#dbeafe,stroke:#1e3a8a,stroke-width:2px,color:#000
    classDef net fill:#e5e7eb,stroke:#374151,stroke-width:2px,color:#000
    classDef ubuntu fill:#fed7aa,stroke:#9a3412,stroke-width:2px,color:#000
    classDef centos fill:#c7d2fe,stroke:#3730a3,stroke-width:2px,color:#000
    class U1,C1 pc
    class U2,C2 net
    class U3,V1 ubuntu
    class C3,V2 centos
    style UBU fill:#fff7ed,stroke:#9a3412,stroke-width:2px,color:#000
    style CEN fill:#eef2ff,stroke:#3730a3,stroke-width:2px,color:#000
    style VMS fill:#fef9c3,stroke:#854d0e,stroke-width:2px,color:#000
    linkStyle 0,1,2,3,4 stroke:#8a8a8a,stroke-width:4px
```

## Komponenten

| Komponente | Rolle | Details |
|---|---|---|
| Oracle VM VirtualBox | Host / Hypervisor | Führt die virtuellen Maschinen aus und isoliert sie |
| Ubuntu Server | Gast-VM 1 | Server der Debian-Familie (`apt`) |
| CentOS | Gast-VM 2 | Server der RHEL-Familie (`yum` / `dnf`) |
| Windows PowerShell | Verwaltungs-Client | Verbindet sich per SSH mit beiden VMs |

## Netzwerkaufbau

| Rechner | Hostname | IP-Adresse | Netzwerkmodus | Zugriff |
|---|---|---|---|---|
| Windows PowerShell (Client) | `my-pc` | `192.168.56.1` | Host-Only-Adapter | entfällt |
| Ubuntu Server | `ubuntu-server` | `192.168.56.101` | Host-Only / NAT | SSH (22) |
| CentOS | `centos-server` | `192.168.56.102` | Host-Only / NAT | SSH (22) |

## Kurzanleitung

Verbindung von PowerShell zu Ubuntu:

```powershell
ssh username@192.168.56.101
```

Verbindung von PowerShell zu CentOS:

```powershell
ssh username@192.168.56.102
```

Verbindung zwischen den beiden VMs prüfen (innerhalb von Ubuntu ausführen):

```bash
ping 192.168.56.102
```

SSH bei Bedarf installieren und aktivieren:

```bash
# Ubuntu
sudo apt update && sudo apt install openssh-server -y
sudo systemctl enable --now ssh

# CentOS
sudo yum install openssh-server -y
sudo systemctl enable --now sshd
```

## Was ich hier übe

- Virtuelle Maschinen in VirtualBox erstellen und verwalten
- Virtuelle Netzwerke konfigurieren (Host-Only, NAT, Bridged)
- Linux-Server aus zwei Distributionsfamilien betreuen (Debian und RHEL)
- Fernadministration per SSH von Windows PowerShell aus
- Paketverwaltung mit `apt` und `yum` / `dnf`
- Dienstverwaltung mit `systemctl`
- Grundlegende Fehlersuche (Konnektivität, Firewall, Ports)

## Screenshots

### Oracle VM VirtualBox Manager

Beide virtuellen Maschinen (`Centos_Server` und `Ubuntu_Server`) laufen in VirtualBox. Das Detailfenster zeigt die VM Ubuntu Server: 2 GB RAM, eine virtuelle Festplatte mit 25 GB und zwei NAT-Adapter.

![Oracle VM VirtualBox Manager mit Centos_Server und Ubuntu_Server](./images/virtualbox.png)

### Terminalsitzungen auf dem CentOS-Server

CentOS Server in VirtualBox: links ein Terminalfenster auf der Windows-Seite, das mit der VM verbunden ist, rechts das Terminal der VM selbst.

![CentOS Server in VirtualBox mit Terminalsitzungen](./images/ssh-session.png)

Verbesserungsvorschläge für das Homelab sind willkommen.

</details>

<details name="language">
<summary><b>English</b></summary>

I run Ubuntu Server and CentOS virtual machines in Oracle VirtualBox and manage them from Windows PowerShell over SSH. The lab is where I practice Linux administration, networking, and remote management.

## Architecture diagram

```mermaid
%%{init: {"themeVariables": {"fontSize": "24px", "edgeLabelBackground": "#ffffff", "textColor": "#000000"}, "flowchart": {"nodeSpacing": 80, "rankSpacing": 80, "padding": 24, "wrappingWidth": 500}}}%%
flowchart TB
    PS["<b>⌨️ Windows PowerShell<br/>SSH Client</b>"]

    subgraph HOST["<b>📦 Oracle VM VirtualBox (Host)</b>"]
        NET{{"<b>🌐 Virtual Network<br/>VMs can ping / SSH each other</b>"}}
        UB["<b>🟠 Ubuntu Server<br/>Guest VM</b>"]
        CE["<b>🔵 CentOS<br/>Guest VM</b>"]
    end

    PS -->|"<b>SSH :22</b>"| NET
    NET --> UB
    NET --> CE

    classDef client fill:#dbeafe,stroke:#1e3a8a,stroke-width:2px,color:#000
    classDef net fill:#e5e7eb,stroke:#374151,stroke-width:2px,color:#000
    classDef ubuntu fill:#fed7aa,stroke:#9a3412,stroke-width:2px,color:#000
    classDef centos fill:#c7d2fe,stroke:#3730a3,stroke-width:2px,color:#000
    class PS client
    class NET net
    class UB ubuntu
    class CE centos
    style HOST fill:#fef9c3,stroke:#854d0e,stroke-width:2px,color:#000
```

## How everything connects

```mermaid
%%{init: {"themeVariables": {"fontSize": "22px"}, "flowchart": {"nodeSpacing": 40, "rankSpacing": 50, "padding": 20, "wrappingWidth": 400}}}%%
flowchart TB
    subgraph UBU["<b>🟠 Step 1 - Connect to Ubuntu</b>"]
        direction LR
        U1["<b>⌨️ PowerShell<br/>ssh to Ubuntu</b>"] --> U2["<b>🌐 VM Network<br/>Port 22</b>"] --> U3["<b>🟠 Ubuntu Server<br/>Shell session opens</b>"]
    end

    subgraph CEN["<b>🔵 Step 2 - Connect to CentOS</b>"]
        direction LR
        C1["<b>⌨️ PowerShell<br/>ssh to CentOS</b>"] --> C2["<b>🌐 VM Network<br/>Port 22</b>"] --> C3["<b>🔵 CentOS<br/>Shell session opens</b>"]
    end

    subgraph VMS["<b>🔁 Step 3 - VM to VM</b>"]
        direction LR
        V1["<b>🟠 Ubuntu Server<br/>ping / ssh</b>"] <--> V2["<b>🔵 CentOS<br/>Reply</b>"]
    end

    UBU ~~~ CEN ~~~ VMS

    classDef pc fill:#dbeafe,stroke:#1e3a8a,stroke-width:2px,color:#000
    classDef net fill:#e5e7eb,stroke:#374151,stroke-width:2px,color:#000
    classDef ubuntu fill:#fed7aa,stroke:#9a3412,stroke-width:2px,color:#000
    classDef centos fill:#c7d2fe,stroke:#3730a3,stroke-width:2px,color:#000
    class U1,C1 pc
    class U2,C2 net
    class U3,V1 ubuntu
    class C3,V2 centos
    style UBU fill:#fff7ed,stroke:#9a3412,stroke-width:2px,color:#000
    style CEN fill:#eef2ff,stroke:#3730a3,stroke-width:2px,color:#000
    style VMS fill:#fef9c3,stroke:#854d0e,stroke-width:2px,color:#000
    linkStyle 0,1,2,3,4 stroke:#8a8a8a,stroke-width:4px
```

## Lab components

| Component | Role | Details |
|---|---|---|
| Oracle VM VirtualBox | Host / hypervisor | Runs and isolates the virtual machines |
| Ubuntu Server | Guest VM #1 | Debian-family server (`apt`) |
| CentOS | Guest VM #2 | RHEL-family server (`yum` / `dnf`) |
| Windows PowerShell | Management client | Connects to both VMs over SSH |

## Network layout

| Machine | Hostname | IP Address | Network Mode | Access |
|---|---|---|---|---|
| Windows PowerShell (client) | `my-pc` | `192.168.56.1` | Host-Only adapter | n/a |
| Ubuntu Server | `ubuntu-server` | `192.168.56.101` | Host-Only / NAT | SSH (22) |
| CentOS | `centos-server` | `192.168.56.102` | Host-Only / NAT | SSH (22) |

## Quick setup notes

Connect from PowerShell to Ubuntu:

```powershell
ssh username@192.168.56.101
```

Connect from PowerShell to CentOS:

```powershell
ssh username@192.168.56.102
```

Check the connection between the two VMs (run from inside Ubuntu):

```bash
ping 192.168.56.102
```

Install and enable SSH if needed:

```bash
# Ubuntu
sudo apt update && sudo apt install openssh-server -y
sudo systemctl enable --now ssh

# CentOS
sudo yum install openssh-server -y
sudo systemctl enable --now sshd
```

## What I practice here

- Creating and managing virtual machines in VirtualBox
- Configuring virtual networking (Host-Only, NAT, Bridged)
- Managing Linux servers across two distro families (Debian and RHEL)
- Remote administration over SSH from Windows PowerShell
- Package management with `apt` and `yum` / `dnf`
- Service management with `systemctl`
- Basic troubleshooting (connectivity, firewall, ports)

## Screenshots

### Oracle VM VirtualBox Manager

Both virtual machines (`Centos_Server` and `Ubuntu_Server`) live in VirtualBox. The details panel shows the Ubuntu Server VM: 2 GB of RAM, a 25 GB virtual disk, and two NAT adapters.

![Oracle VM VirtualBox Manager with Centos_Server and Ubuntu_Server](./images/virtualbox.png)

### Terminal sessions on CentOS Server

CentOS Server running in VirtualBox, with a terminal window from the Windows side (left) connected to the VM next to the VM's own terminal (right).

![CentOS Server running in VirtualBox with terminal sessions](./images/ssh-session.png)

Suggestions for improving the lab are welcome.

</details>
