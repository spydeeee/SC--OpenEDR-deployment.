# SC-OpenEDR-deployment.
# 🛡️ Endpoint Detection and Response (EDR) Home Lab — Xcitium OpenEDR

## Objective
Deploy and configure a cloud-managed **Xcitium OpenEDR** environment to monitor Windows endpoints in real time , enroll target hosts using the OpenEDR Communication Agent, remotely push Endpoint Security and Extended Detection and Response (EXDR) packages, and perform endpoint threat hunting, hash validation, and incident triage .

Unlike traditional signature-based Antivirus (AV) solutions that focus strictly on prevention, an EDR platform continuously collects endpoint telemetry to detect, investigate, and respond to both known and unknown threats.

---

## Skills Learned
- Differentiating between traditional signature-based Antivirus (AV) prevention and real-time behavioral Endpoint Detection and Response (EDR/EXDR) .
- Provisioning a cloud-based OpenEDR management console and establishing baseline security monitoring dashboards
- Enrolling multiple Windows endpoints (`DESKTOP-OPOUVV6` and `LAPTOP-JRRT9IHB`) via the Endpoint Manager Communication Agent].
- Remotely deploying security packages (**Xcitium Client - Security** and **Xcitium Client - EXDR v2.13.0.161**) and monitoring MSI installation queues across managed assets.
- Investigating endpoint process execution events, extracting SHA-256 file hashes, and validating indicators of compromise (IoCs) against VirusTotal .
- Performing remote endpoint administration, patch status verification, and remote reboot/containment workflows.

---

## Tools Used
- **Xcitium OpenEDR Platform**: Cloud-hosted security management console for endpoint visibility, alert triage, and package deployment.
- **Xcitium Endpoint Manager (Communication Client)**: Lightweight agent  establishing telemetry and command communication between managed Windows hosts and the cloud portal.
- **Xcitium Client - Security & EXDR (v2.13.0.161)**: Endpoint antivirus, firewall, containment, and extended detection and response modules.
- **Windows Endpoints (`DESKTOP-OPOUVV6` & `LAPTOP-JRRT9IHB`)**: Target workstations enrolled in the home lab for agent deployment and telemetry collection.
- **VirusTotal**: Threat intelligence service used to verify file hashes captured by OpenEDR event logs.

---

## Steps

### Step 1: Provisioning the Xcitium OpenEDR Console & Baseline Verification
1. Registered and logged into the **Xcitium OpenEDR Platform** (`openedr.platform.xcitium.com`).
2. Navigated to **Dashboards > Service** (`openedr.platform.xcitium.com/dashboards/service-summary-mdr`) to verify the initial baseline state of the <img width="1920" height="1080" alt="Screenshot (21)" src="https://github.com/user-attachments/assets/294a4dda-9d6f-45ef-a975-fb04f213105d" />



3. Opened **Dashboards > Endpoint** to launch the four-stage onboarding workflow (**Welcome → Enroll Devices → Deploy EDR → You're Ready!**

4. In the **Enroll Devices** prompt, copied the unique Windows Communication Agent enrollment link (`https://<tenant>.itsm-us1.comodo.com:443/enroll/...`) <img width="1920" height="1080" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/4ba7480d-7dbf-44a5-99d0-87b2ae03af60" />



---

### Step 2: Downloading & Installing the Communication Agent
1. Opened the enrollment URL on the target Windows machines to launch the **Endpoint Manager Enrollment Wizard**.
2. Clicked **Download Windows Installer** under the **Installer** section.
3. Followed the two-step installation procedure:
   - **Step 1**: Executed the Communication Client installer on each Windows host after the download completed.
   - **Step 2**: Verified that each device completed registration with the cloud portal.

<img width="1920" height="1080" alt="Screenshot (19)" src="https://github.com/user-attachments/assets/72a77659-0446-4282-ab6b-ec7bbfe4974d" />

---

### Step 3: Verifying Enrolled Endpoints in Device Management
1. Navigated to **Management > Device Management > Device List** (`openedr.platform.xcitium.com/assets/endpoint/device-list`) under `Default Customer'.
2. Confirmed that both Windows endpoints (`DESKTOP-OPOUVV6` and `LAPTOP-JRRT9IHB`) successfully checked in under the `FREE`.
3. Reviewed the **ACTIVE COMPONENTS** column: at this stage, only the **`AG`** (Communication Agent) badge was active in green, while `XCS`, `EDR`, `EXDR`, and `MDR` remained inactive pending package deployment.

<img width="1920" height="1080" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/0c246bf3-2c20-4ad2-ac81-00831874c0ad" />

---

### Step 4: Deploying Additional Security & EXDR Packages
1. Selected the target workstation **`DESKTOP-OPOUVV6`** from the **Device List** and clicked **Install or Manage Packages > Install Additional Xcitium Packages**.
2. Configured the package deployment options in the modal window:
   - **`Install Xcitium Client - Security`**: Provides antivirus, firewall, and containment protection.
   - **`Install Xcitium Client - EXDR`**: Deploys the Extended Detection and Response sensor for real-time behavioral monitoring and telemetry.
3. Under **Reboot options**, selected **Warn about the reboot and let users postpone it** with the administrator prompt: *"Your device needs a reboot because it's required by your administrator"*, and clicked **Install** to push the command to the endpoint.

<img width="1920" height="1080" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/6a00ba04-2fe4-4615-9ff0-baae57952331" />

---

### Step 5: Monitoring the MSI Installation Queue
1. Inside the **`DESKTOP-OPOUVV6`** device management view, navigated to the **MSI Installation State** tab to monitor remote deployment progress.
2. Verified under **Log Details > Statuses** (`2026/10/14 03:11:41 AM`) that **`Xcitium Extended Detection and Response v. 2.13.0.161 MSI Installation in the queue.`** was actively queued and downloading to the host.
<img width="1920" height="1080" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/b0ce58e1-4612-4f4d-bf8c-be86e6936879" />

---

### Step 6: Endpoint Telemetry Verification, Threat Hunting & Remote Response
1. **Verify Active Protection**: Once the MSI installation completed and the endpoint rebooted, confirmed that the active components updated to show the EDR and Security modules enabled alongside the agent.
2. **Endpoint Security Telemetry**: Navigated to **Security > Endpoint Security** to inspect real-time process execution events and binary activity in temporary directories across enrolled hosts.
3. **Hash Investigation & Threat Hunting**:
   - Opened **Investigate > Hash Search / Events** to review process activity on the monitored machine.
   - Extracted file hashes from running system processes (such as `conhost.exe`) and cross-referenced them on **VirusTotal** to validate whether binaries were benign or malicious .
4. **Alert Triage & Remote Administration**:
   - Reviewed **Investigate > Alarm Search** and **Device View** to filter security alerts and administrative privilege usage per host.
   - Tested **Remote Control** (protected with MFA) and **Patch Management** to remotely administer the endpoint and apply pending Windows updates.

---

## References
- **Video Guide**: [What is Endpoint Detection and Response (EDR) | Get Xcitium OpenEDR FREE Today! (InfoSec Pat)](http://www.youtube.com/watch?v=MI5DMKuxhH0) 
- **Portfolio Template**: [MyDFIR Detection-Lab Template](https://github.com/Test-MyDFIR/Detection-Lab/blob/main/README.md)
