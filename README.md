# Security Audit: BS Producten Petcam

## 1. Overview
Findings for the BS Producten Petcam (Firmware 33.1.0.0818). A chain of vulnerabilities allows unauthenticated root access via proximity or LAN.

---

## 2. Technical Background
Analysis performed via SPI flash dump and QEMU emulation.

### Attack Surface
In "Local Mode," the device acts as a gateway with the following open ports:
* **80 (HTTP):** Web server
* **554 (RTSP):** Video stream
* **8001 (API):** Custom P2P service (Exploitation target)

### Hardcoded Credentials
* **System Root:** `root:cxlinux`
* **ONVIF/API:** `admin:12345678`
* **Static Keys:** Encryption strings like `a9m0d3enckEy$k3y` found in binaries.

### Unauthenticated Video Stream
The RTSP server on port 554 requires no credentials. The live feed is accessible via VLC:
`rtsp://<IP>:554/` 

---

## 3. Vulnerability Summary

| ID | Type | Severity | Description |
| :--- | :--- | :--- | :--- |
| **[CVE-2025-69988](./CVE-2025-69988.md)** | Network | Medium | Unauthenticated "Local Mode" Wi-Fi AP. |
| **[CVE-2024-51348](./CVE-2024-51348.md)** | Buffer overflow | High | Stack-based Buffer Overflow (RCE) on Port 8001. |
