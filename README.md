# SC-OpenEDR-deployment.
# 🛡️ Endpoint Detection and Response (EDR) Home Lab — Xcitium OpenEDR

## Objective
Deploy and configure a cloud-managed **Xcitium OpenEDR** environment to monitor Windows endpoints in real time [00:00:48], enroll target hosts using the OpenEDR Communication Agent[cite: 1, 2], remotely push Endpoint Security and Extended Detection and Response (EXDR) packages[cite: 5, 6], and perform endpoint threat hunting, hash validation, and incident triage [00:11:57].

Unlike traditional signature-based Antivirus (AV) solutions that focus strictly on prevention, an EDR platform continuously collects endpoint telemetry to detect, investigate, and respond to both known and unknown threats [00:03:44].

---

## Skills Learned
- Differentiating between traditional signature-based Antivirus (AV) prevention and real-time behavioral Endpoint Detection and Response (EDR/EXDR) [00:03:44].
- Provisioning a cloud-based OpenEDR management console and establishing baseline security monitoring dashboards[cite: 1, 4].
- Enrolling multiple Windows endpoints (`DESKTOP-OPOUVV6` and `LAPTOP-JRRT9IHB`) via the Endpoint Manager Communication Agent (`AG`)[cite: 2, 3, 5].
- Remotely deploying security packages (**Xcitium Client - Security** and **Xcitium Client - EXDR v2.13.0.161**) and monitoring MSI installation queues across managed assets[cite: 5, 6].
- Investigating endpoint process execution events, extracting SHA-256 file hashes, and validating indicators of compromise (IoCs) against VirusTotal [00:11:57].
- Performing remote endpoint administration, patch status verification, and remote reboot/containment workflows [00:14:18].

---

## Tools Used
- **Xcitium OpenEDR Platform**: Cloud-hosted security management console for endpoint visibility, alert triage, and package deployment[cite: 1, 4].
- **Xcitium Endpoint Manager (Communication Client)**: Lightweight agent (`AG`) establishing telemetry and command communication between managed Windows hosts and the cloud portal [00:04:42][cite: 2, 3].
- **Xcitium Client - Security & EXDR (v2.13.0.161)**: Endpoint antivirus, firewall, containment, and extended detection and response modules [00:02:48][cite: 5, 6].
- **Windows Endpoints (`DESKTOP-OPOUVV6` & `LAPTOP-JRRT9IHB`)**: Target workstations enrolled in the home lab for agent deployment and telemetry collection[cite: 3, 5].
- **VirusTotal**: Threat intelligence service used to verify file hashes captured by OpenEDR event logs [00:12:36].

---

## Steps

### Step 1: Provisioning the Xcitium OpenEDR Console & Baseline Verification
1. Registered and logged into the **Xcitium OpenEDR Platform** (`openedr.platform.xcitium.com`) [00:01:55][cite: 1].
2. Navigated to **Dashboards > Service** (`openedr.platform.xcitium.com/dashboards/service-summary-mdr`) to verify the initial baseline state of the **Service Summary** prior to endpoint ingestion, confirming `0` Events Processed, `0` Investigated Alerts, `0` Incidents, `0` Tickets Notified, and `0` Unread Reports[cite: 4].

![Service Summary Dashboard Baseline](images/Screenshot%20(21).png)

3. Opened **Dashboards > Endpoint** to launch the four-stage onboarding workflow (**Welcome → Enroll Devices → Deploy EDR → You're Ready!**)[cite: 1].
4. In the **Enroll Devices** prompt, copied the unique Windows Communication Agent enrollment link (`https://<tenant>.itsm-us1.comodo.com:443/enroll/...`) [00:05:33][cite: 1].

![OpenEDR Onboarding and Device Enrollment Link](images/Screenshot%20(18).png)

---

### Step 2: Downloading & Installing the Communication Agent
1. Opened the enrollment URL on the target Windows machines to launch the **Endpoint Manager Enrollment Wizard**[cite: 2].
2. Clicked **Download Windows Installer** under the **Installer** section [00:03:33][cite: 2].
3. Followed the two-step installation procedure:
   - **Step 1**: Executed the Communication Client installer on each Windows host after the download completed [00:04:19][cite: 2].
   - **Step 2**: Verified that each device completed registration with the cloud portal[cite: 2].

![Endpoint Manager Enrollment Wizard](images/Screenshot%20(19).png)

---

### Step 3: Verifying Enrolled Endpoints in Device Management
1. Navigated to **Management > Device Management > Device List** (`openedr.platform.xcitium.com/assets/endpoint/device-list`) under `Default Customer` [00:04:31][cite: 3].
2. Confirmed that both Windows endpoints (`DESKTOP-OPOUVV6` and `LAPTOP-JRRT9IHB`) successfully checked in under the `FREE` policy[cite: 3, 5].
3. Reviewed the **ACTIVE COMPONENTS** column: at this stage, only the **`AG`** (Communication Agent) badge was active in green, while `XCS`, `EDR`, `EXDR`, and `MDR` remained inactive pending package deployment [00:04:42][cite: 3].

![Device List Showing Enrolled Windows Endpoints](images/Screenshot%20(20).png)

---

### Step 4: Deploying Additional Security & EXDR Packages
1. Selected the target workstation **`DESKTOP-OPOUVV6`** from the **Device List** and clicked **Install or Manage Packages > Install Additional Xcitium Packages** [00:06:17][cite: 5, 6].
2. Configured the package deployment options in the modal window:
   - **`Install Xcitium Client - Security`**: Provides antivirus, firewall, and containment protection [00:02:48][cite: 6].
   - **`Install Xcitium Client - EXDR`**: Deploys the Extended Detection and Response sensor for real-time behavioral monitoring and telemetry [00:06:17][cite: 6].
3. Under **Reboot options**, selected **Warn about the reboot and let users postpone it** with the administrator prompt: *"Your device needs a reboot because it's required by your administrator"*, and clicked **Install** to push the command to the endpoint [00:06:35][cite: 6].

![Install Additional Xcitium Packages Modal](images/Screenshot%20(23).png)

---

### Step 5: Monitoring the MSI Installation Queue
1. Inside the **`DESKTOP-OPOUVV6`** device management view, navigated to the **MSI Installation State** tab to monitor remote deployment progress [00:06:49][cite: 5].
2. Verified under **Log Details > Statuses** (`2026/10/14 03:11:41 AM`) that **`Xcitium Extended Detection and Response v. 2.13.0.161 MSI Installation in the queue.`** was actively queued and downloading to the host [00:10:15][cite: 5].

![MSI Installation State Log Details](images/Screenshot%20(22).png)

---

### Step 6: Endpoint Telemetry Verification, Threat Hunting & Remote Response
1. **Verify Active Protection**: Once the MSI installation completed and the endpoint rebooted, confirmed that the active components updated to show the EDR and Security modules enabled alongside the agent [00:10:41].
2. **Endpoint Security Telemetry**: Navigated to **Security > Endpoint Security** to inspect real-time process execution events and binary activity in temporary directories across enrolled hosts [00:11:15].
3. **Hash Investigation & Threat Hunting**:
   - Opened **Investigate > Hash Search / Events** to review process activity on the monitored machine [00:11:57].
   - Extracted file hashes from running system processes (such as `conhost.exe`) and cross-referenced them on **VirusTotal** to validate whether binaries were benign or malicious [00:12:36].
4. **Alert Triage & Remote Administration**:
   - Reviewed **Investigate > Alarm Search** and **Device View** to filter security alerts and administrative privilege usage per host [00:13:05].
   - Tested **Remote Control** (protected with MFA) and **Patch Management** to remotely administer the endpoint and apply pending Windows updates [00:14:18].

---

## References
- **Video Guide**: [What is Endpoint Detection and Response (EDR) | Get Xcitium OpenEDR FREE Today! (InfoSec Pat)](http://www.youtube.com/watch?v=MI5DMKuxhH0) [00:00:00]
- **Portfolio Template**: [MyDFIR Detection-Lab Template](https://github.com/Test-MyDFIR/Detection-Lab/blob/main/README.md)
