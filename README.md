# 🧠 Monitoring Dashboard (Prometheus + Grafana + Node Exporter)

A lightweight monitoring stack built with **Docker Compose**, featuring:
- **Prometheus** for metrics collection  
- **Grafana** for visualization  
- **Node Exporter** for system metrics  

This setup gives you a real-time dashboard to monitor system health and trigger alerts — perfect for DevOps portfolios and production use.

---

## 🧩 Stack Overview

| Service | Purpose | Port |
|----------|----------|------|
| Prometheus | Metrics collection & alerting | `9090` |
| Grafana | Dashboards & visualizations | `3000` |
| Node Exporter | System metrics exporter | `9100` |

---

## ⚙️ Folder Structure

MONITORING-DASHBOARD/
├── grafana/
│   ├── provisioning/
│   │   ├── dashboards/
│   │   │   └── dashboards.yml
│   │   └── datasources/
│   │       └── datasources.yml
│   └── dashboards/
│       └── system-metrics.json
├── prometheus/
│   ├── prometheus.yml
│   └── alerts.yml
├── .env
└── docker-compose.yml

yaml
Copy code

---

## 🚀 Getting Started

### 1️⃣ Clone the repository
```bash
git clone https://github.com/<your-username>/monitoring-dashboard.git
cd monitoring-dashboard
2️⃣ Configure environment variables
Edit .env file (or create one):

env
Copy code
GRAFANA_USER=admin
GRAFANA_PASSWORD=admin
3️⃣ Start the stack
bash
Copy code
docker compose up -d
This will start:

Prometheus on http://localhost:9090

Grafana on http://localhost:3000

🧠 Access the Dashboards
Go to http://localhost:3000

Log in with the credentials from .env

Navigate to Dashboards → Manage → System Metrics

You should see real-time CPU, memory, and disk usage data from Node Exporter

⚠️ Testing Alerts (High CPU Usage)
To simulate high CPU load on macOS:

bash
Copy code
yes > /dev/null
This will max one CPU core.
Stop with:

bash
Copy code
killall yes
Your Prometheus alert (defined in prometheus/alerts.yml) should trigger when CPU > 80% for 5 minutes.

🧰 GitHub Actions (CI/CD)
This project can integrate with GitHub Actions to:

Build and test Docker images

Deploy automatically to Fly.io or other platforms

To enable this, add a workflow file in:

bash
Copy code
.github/workflows/deploy.yml
🧹 Common Issues
❌ Error: “Are you trying to mount a directory onto a file?”
This means your prometheus.yml or alerts.yml path is incorrect.
Double-check that the files exist in ./prometheus/ and match the paths in docker-compose.yml.

📈 Future Enhancements
Add Alertmanager for email/Slack notifications

Add cAdvisor for container-level metrics

Add Blackbox Exporter for endpoint uptime monitoring

🧑‍💻 Author
Philip Nwachukwu
DevOps Engineer • Cloud Enthusiast
🌐 your-portfolio-link.com

🪪 License
MIT License © 2025 Philip Nwachukwu

yaml
Copy code

---

Would you like me to include a **GitHub Actions deploy workflow** section at the bottom of this README (with sample YAML for building and deploying to Fly.io)?  
That’ll make your project look *production-ready*.