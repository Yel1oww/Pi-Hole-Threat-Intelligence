# 🛡️ Pi-hole Threat Intelligence & Auto-Blocking

Automated security pipeline that monitors Pi-hole DNS queries, cross-references them with global threat feeds (AlienVault & VirusTotal), automatically blocks malicious domains, and visualizes everything in Grafana.

---

## 🚀 Overview
This project creates a closed-loop security system for your home network:
1.  **Extract:** n8n pulls logs from your **Pi-hole** every 5 minutes.
2.  **Analyze:** Domains are checked against **AlienVault OTX** and **VirusTotal**.
3.  **Act:** If a domain exceeds a threat threshold (e.g., >10 VT detections), n8n sends a command to Pi-hole to **immediately block** it.
4.  **Log:** All scan results are stored in **InfluxDB**.
5.  **Visualize:** A **Grafana** dashboard displays real-time threat levels and block history.

## 🛠️ Prerequisites
* **Pi-hole** (with API access enabled)
* **n8n** (Self-hosted or Cloud)
* **InfluxDB 2.x** (For data storage)
* **Grafana** (For the dashboard)
* **API Keys:** * [AlienVault OTX](https://otx.alienvault.com/) (Free)
    * [VirusTotal](https://www.virustotal.com/) (Free tier allows 4 requests/min)

---

## 📦 Installation

### 1. n8n Workflow
1.  Open your n8n instance and create a **New Workflow**.
2.  Open the `n8n_workflow.json` file from this repo.
3.  Copy the content and paste it directly onto the n8n canvas.
4.  **Configuration Required:**
    * Update the **HTTP Request** nodes with your Pi-hole and InfluxDB IP addresses.
    * Add your API keys to the header parameters for VirusTotal and AlienVault nodes.
    * Update the InfluxDB node with your Organization, Bucket, and Token.

### 2. InfluxDB Setup
Create a bucket named `DNS_Threats` (or your preferred name) in your InfluxDB UI. Ensure your API token has **Write** permissions for this bucket.

### 3. Grafana Dashboard
1.  In Grafana, go to **Dashboards** > **New** > **Import**.
2.  Upload the `grafana_dashboard.json` file or paste the JSON text.
3.  Select your **InfluxDB** data source from the dropdown.
4.  If your bucket name is not `AI`, you may need to find and replace the bucket name in the panel queries.

---

## ⚙️ Configuration Variables
When setting up the JSONs, ensure you replace the following placeholders:

| Placeholder | Description |
| :--- | :--- |
| `[YOUR_PIHOLE_IP]` | The local IP of your Pi-hole instance. |
| `[YOUR_PIHOLE_AUTH_TOKEN]` | Found in `/etc/pihole/setupVars.conf` (WEBPASSWORD). |
| `[YOUR_INFLUXDB_IP]` | The IP where your InfluxDB is running. |
| `[YOUR_ORG_NAME]` | Your InfluxDB Organization name. |
| `[YOUR_BUCKET_NAME]` | The name of the bucket used for storage. |
| `[YOUR_VIRUSTOTAL_API_KEY]` | Your personal VirusTotal API Key. |
| `[YOUR_ALIENVAULT_API_KEY]` | Your personal AlienVault OTX API Key. |

---

## 🤝 Contributing
Feel free to fork this repository and submit pull requests. For major changes, please open an issue first to discuss what you would like to change.

## 📄 License
[MIT](https://choosealicense.com/licenses/mit/)
