## 🎯 Phase 1: Phishing Page Setup with SET on Kali Linux

In this phase, I used the Social-Engineer Toolkit (SET) to simulate a phishing attack. I cloned a real login page and captured credentials submitted by a victim.
> 🧪 **Lab Goal**: Simulate phishing, capture credentials with SET, then forward the data to Splunk for detection and analysis.

---

### 🔧 Step 1: Stop Apache & Launch SET

```bash
sudo apt update
sudo apt install apache2 -y     # Apache was already installed
sudo service apache2 stop       # Stopped Apache to free up port 80
sudo setoolkit
```

📸 **Screenshot 1**: SET interface launched successfully
<img width="1278" height="798" alt="VirtualBox_Kali Linux_16_07_2025_00_16_01" src="https://github.com/user-attachments/assets/45c759b6-9d36-4a0d-8a57-a951ac6ac61f" />

---

### ⚙️ Step 2: Clone a Target Website in SET

From the SET menu, I followed this path:

- 1) Social-Engineering Attacks  
- 2) Website Attack Vectors  
- 3) Credential Harvester Attack Method  
- 2) Site Cloner  
- IP address: `192.168.117.4`  
- URL to clone: `http://testphp.vulnweb.com/login.php`

SET cloned the site and started listening on port 80.

📸 **Screenshot 2**: Cloned page hosted locally
<img width="1278" height="798" alt="VirtualBox_Kali Linux_16_07_2025_01_03_48" src="https://github.com/user-attachments/assets/229fce28-da98-4d39-878d-1e580741839b" />

---

### 🧪 Step 3: Test Victim Login to Capture Credentials

I visited the fake site from another machine and submitted fake login credentials:

- **Username**: `jmcoded`  
- **Password**: `Passswordjmcoded109754`

SET captured the POST request with the credentials:

```
[*] WE GOT A HIT!
uname=jmcoded
pass=Passswordjmcoded109754
```

📸 **Screenshot 3**: Victim login page  <img width="1920" height="1010" alt="image" src="https://github.com/user-attachments/assets/80422d58-6050-455d-a494-c8b978507b77" />

📸 **Screenshot 4**: Captured credentials in SET <img width="1278" height="798" alt="VirtualBox_Kali Linux_16_07_2025_01_06_51" src="https://github.com/user-attachments/assets/8effd549-a31a-4036-8394-00894876aea8" />


---

### ✅ Phase Summary

- Apache was stopped to allow SET to bind to port 80.
- SET cloned a login page and captured user-submitted credentials.
- This phase simulated a phishing attack in a safe, isolated lab.
- 
## 🧠 Phase 2: Sending Captured Phishing Credentials to Splunk (HEC)

In this phase, I simulated how phishing logs (like stolen usernames and passwords) can be sent to a centralized log platform like Splunk using the HTTP Event Collector (HEC). This replicates what happens in real-world SOC pipelines.

---

### 🟢 Step 1: Start Splunk & Confirm HEC is Enabled

I started my local Splunk instance on Kali using the terminal:

```bash
sudo /opt/splunk/bin/splunk start
```

Then I logged into Splunk via browser at:  
**http://localhost:8000**

Inside Splunk, I navigated to:

- **Settings** → **Data Inputs** → **HTTP Event Collector**
- Enabled HEC if it wasn’t on
- Created a new token called `phishing-token` with index: `main`

📸 **Screenshot 5**: HEC token interface on Splunk
<img width="1278" height="798" alt="VirtualBox_Kali Linux_16_07_2025_01_42_16" src="https://github.com/user-attachments/assets/b05312b0-3fad-4582-8e78-0451b1e9ebf1" />

---

### 🟢 Step 2: Send the Phishing Data via cURL

Now I simulated sending the stolen credentials from the phishing site (Phase 1) into Splunk using this `curl` command:

```bash
curl -k https://localhost:8088/services/collector/event \
 -H "Authorization: Splunk 8d71c154-80ef-47ff-b2e8-b9b6cbe1c386" \
 -d '{"event": "uname=jmcoded&pass=Passswordjmcoded109754", "sourcetype": "phishing_web_log"}'
```

If successful, I got a response like:

```json
{"text":"Success","code":0}
```

📸 **Screenshot 6**: Curl request showing success response from Splunk
<img width="1278" height="798" alt="VirtualBox_Kali Linux_16_07_2025_01_43_44" src="https://github.com/user-attachments/assets/e72f483a-fc13-4a69-82b9-cf98c070ed8f" />

---

### 🟢 Step 3: Search the Logs in Splunk

To verify the log made it into Splunk, I ran this SPL search:

```spl
index=* sourcetype=phishing_web_log
```

The log entry appeared with the exact credentials captured by SET.

📸 **Screenshot 7**: Splunk search result showing captured phishing data
<img width="1920" height="909" alt="VirtualBox_Kali Linux_16_07_2025_01_46_12" src="https://github.com/user-attachments/assets/925afdb7-5235-4596-a660-12978b202d4e" />

---

### ✅ Phase Summary

- I started Splunk and confirmed HTTP Event Collector was active.
- Simulated log forwarding from SET using curl and the HEC token.
- Verified the phishing credentials were successfully received by Splunk.

## 📊 Phase 3: Visualizing Phishing Logs in Splunk

In this phase, I visualized the captured phishing credentials using Splunk dashboards. The goal was to understand how logs from a phishing campaign can be transformed into searchable, visual data for SOC analysts.

---

### 📥 Step 1: Send Multiple Fake Credentials into Splunk

To simulate real phishing traffic, I sent multiple login attempts using `curl` with different fake usernames, passwords, and IPs. Here's an example:

```bash
curl -k https://localhost:8088/services/collector/event \
 -H "Authorization: Splunk 8d71c154-80ef-47ff-b2e8-b9b6cbe1c386" \
 -d '{"event": {"username": "user20", "password": "Pass20", "ip": "192.168.117.20", "browser": "Chrome", "login_page": "testphp.vulnweb.com/login.php"}, "sourcetype": "phishing_web_log"}'
```

I repeated this with different values to generate multiple events in Splunk.

📸 **Screenshot 8**: Sending multiple simulated phishing credentials into Splunk  
<img width="1038" height="798" alt="VirtualBox_Kali Linux_16_07_2025_02_58_05" src="https://github.com/user-attachments/assets/15d43187-5418-4ecd-8307-fa423b6f2509" />

---

### 🔍 Step 2: Search Logs Using SPL

To view the phishing data, I used this SPL:

```spl
index=* sourcetype=phishing_web_log
```

This showed all the events with usernames, passwords, IPs, browsers, and the login page used.

📸 **Screenshot 9**: Raw phishing event logs in Splunk  
<img width="1920" height="909" alt="VirtualBox_Kali Linux_16_07_2025_02_59_48" src="https://github.com/user-attachments/assets/d1ea4e38-a426-4ce0-b467-3afa3ec17b59" />

---

### 📈 Step 3: Generate Visual Insights from the Data

I built simple visualizations directly from the search bar using `stats` commands. Example:

```spl
index=* sourcetype=phishing_web_log
| stats count by ip
| sort -count
```

This returned a chart showing which IP address submitted the most phishing attempts.

📸 **Screenshot 10**: IP distribution chart from phishing logs  
<img width="1920" height="909" alt="VirtualBox_Kali Linux_16_07_2025_03_00_42" src="https://github.com/user-attachments/assets/3cf61381-5219-4fd7-8c65-6c45f514350b" />

---

### ✅ Phase Summary

- Simulated a phishing campaign with 20+ fake login attempts.
- Indexed data into Splunk using HEC with JSON events.
- Visualized login patterns by IP, usernames, and browsers.
- This phase showed how phishing data can be used for threat intel and user behavior analysis.

## 🚨 Phase 4: Detect and Harden Against Phishing

In this final phase, I moved from detection to response. I created a Splunk alert to catch phishing activity and then simulated defensive hardening by blocking the phishing server IP with `iptables`.

---

### 🔍 Step 1: Create a Splunk Alert

To catch phishing attempts, I created a custom Splunk alert using this search:
```spl
index=main sourcetype=phishing_web_log "uname=" | table ip, username, _time
```
**Alert Settings:**
- **Name**: `Phishing_Credential_Detection`
- **Trigger**: When results > 0 within 5 minutes
- **Action**: Log to Splunk (simulating a SOC alert)

📸 **Screenshot 11**: Splunk alert configuration  
<img width="1038" height="798" alt="VirtualBox_Kali Linux_16_07_2025_03_57_32" src="https://github.com/user-attachments/assets/81081e32-2e01-4518-91ae-d59e2a81ee14" />

---
### 🛡️ Step 2: Block the Phishing Server with iptables

To simulate a response, I blocked the phishing site IP:

```spl
sudo iptables -A INPUT -s 192.168.117.4 -j DROP
```
Then I tested access from the same Kali system:

```spl
curl http://192.168.117.4
```
The request failed with a timeout error — proof that the IP was successfully blocked.

📸 **Screenshot 12**: iptables rule applied  
<img width="1038" height="798" alt="image" src="https://github.com/user-attachments/assets/eb8e740b-45d0-419d-a116-4c2e595e0c76" />

---

### ✅ Phase Summary

- ✅ Created a Splunk alert to detect phishing credential submissions in logs.
- ✅ Simulated hardening by blocking the attacker’s IP address.
- ✅ Demonstrated detection and response workflow from a SOC analyst perspective.


---

## 🧠 Final Thoughts

This lab helped me connect offensive and defensive concepts in one project — from simulating phishing attacks to detecting and responding like a SOC analyst. I learned how phishing campaigns are built using tools like SET, how to centralize logs with Splunk HEC, and how to write SPL queries for real-time visibility. I also simulated an actual response by blocking the attacker’s IP using `iptables`.

This hands-on project sharpened my red teaming, blue teaming, and threat detection skills — all in one lab.



Thanks for reading!


