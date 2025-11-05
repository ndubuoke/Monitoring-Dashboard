# System Monitoring Dashboard

A complete monitoring solution using Prometheus, Grafana, and Node Exporter to track system metrics with real-time alerts.

![Monitoring Stack](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

## 📊 What This Monitors

- **CPU Usage**: Real-time CPU utilization percentage
- **Memory Usage**: RAM consumption and availability
- **Alerts**: Automated notifications when CPU exceeds 5%

## 🏗️ Architecture

```
┌─────────────────┐
│  Node Exporter  │  ──→  Collects system metrics
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│   Prometheus    │  ──→  Stores & queries metrics
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│    Grafana      │  ──→  Visualizes data
└─────────────────┘
```

## 🚀 Quick Start

### Prerequisites

- Docker & Docker Compose installed
- Ports 3000, 9090, and 9100 available

### 1. Clone and Setup

```bash
git clone <your-repo-url>
cd monitoring-dashboard
```

### 2. Create Environment File

Create a `.env` file in the root directory:

```bash
GRAFANA_USER=admin
GRAFANA_PASSWORD=your_secure_password
```

### 3. Project Structure

Ensure your folder structure looks like this:

```
monitoring-dashboard/
├── docker-compose.yml
├── .env
├── grafana/
│   ├── provisioning/
│   │   ├── dashboards/
│   │   │   └── dashboards.yml
│   │   └── datasources/
│   │       └── datasources.yml
│   └── dashboards/
│       └── system-metrics.json
└── prometheus/
    ├── prometheus.yml
    └── alerts.yml
```

### 4. Start the Stack

```bash
# Start all services
docker-compose up -d

# Check status
docker-compose ps

# View logs
docker logs grafana
docker logs prometheus
```

### 5. Access the Applications

- **Grafana Dashboard**: http://localhost:3000
  - Username: `admin`
  - Password: (from your `.env` file)
  
- **Prometheus**: http://localhost:9090
  - View metrics and alerts
  
- **Node Exporter**: http://localhost:9100/metrics
  - Raw system metrics

## 📁 Configuration Files

### Docker Compose (`docker-compose.yml`)

Orchestrates three services:
- **Prometheus**: Metrics collection and storage
- **Grafana**: Visualization and dashboards
- **Node Exporter**: System metrics exporter

### Prometheus Configuration (`prometheus/prometheus.yml`)

Defines scrape targets and alert rules:
```yaml
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['node_exporter:9100']
```

### Alert Rules (`prometheus/alerts.yml`)

CPU usage alerts:
- **Warning**: Fires when CPU > 5% for 1 minute
- **Critical**: Fires when CPU > 80% for 2 minutes

### Grafana Dashboard (`grafana/dashboards/system-metrics.json`)

Pre-configured dashboard with:
- CPU usage graph with 5% threshold line
- Memory usage graph
- Active alerts panel
- Alert history table

## 🔔 Testing Alerts

### Generate CPU Load

```bash
# Start CPU-intensive process
yes > /dev/null &

# For multi-core systems, run multiple instances
yes > /dev/null &
yes > /dev/null &
yes > /dev/null &
```

### Monitor Alert Status

1. **Prometheus Alerts**: http://localhost:9090/alerts
   - Watch status change: Inactive → Pending → Firing

2. **Grafana Dashboard**: http://localhost:3000/d/system-metrics
   - See CPU spike above 5% threshold
   - Active Alerts panel turns red

### Stop Test

```bash
killall yes
```

## 🛠️ Troubleshooting

### Dashboard Not Showing

```bash
# Check if Grafana can see the provisioning files
docker exec -it grafana ls -la /etc/grafana/provisioning/dashboards/

# Restart Grafana
docker-compose restart grafana
```

### No Metrics in Dashboard

```bash
# Verify Node Exporter is running
curl http://localhost:9100/metrics

# Check Prometheus targets
# Go to: http://localhost:9090/targets
# Both prometheus and node_exporter should show "UP" (green)
```

### Alerts Not Firing

```bash
# Check alert rules are loaded
# Go to: http://localhost:9090/alerts

# Verify CPU query returns data in Prometheus
# Query: 100 - (avg by(instance)(irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

### View Logs

```bash
# Grafana logs
docker logs grafana

# Prometheus logs
docker logs prometheus

# Node Exporter logs
docker logs node_exporter
```

## 🔧 Customization

### Change Alert Threshold

Edit `prometheus/alerts.yml`:

```yaml
- alert: HighCPUUsage
  expr: 100 - (avg by(instance)(irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 10  # Change 5 to 10
  for: 2m  # Change duration
```

Then restart Prometheus:
```bash
docker-compose restart prometheus
```

### Add More Metrics

Edit `grafana/dashboards/system-metrics.json` to add panels for:
- Disk usage
- Network I/O
- System load
- Process counts

### Modify Dashboard Refresh Rate

In `system-metrics.json`, change:
```json
"refresh": "5s"  // Change to "10s", "30s", "1m", etc.
```

## 🌐 Deploying to Production

### Fly.io Deployment

When deployed to Fly.io, the system monitors the Fly.io VM running your containers.

**Important adjustments needed:**

1. Update `prometheus/prometheus.yml` for Fly.io networking
2. Add persistent volumes for Prometheus data
3. Configure proper resource limits
4. Set up external alerting (email, Slack, PagerDuty)

### Security Considerations

- Change default Grafana password
- Enable HTTPS/TLS
- Restrict access with authentication
- Use secrets management for credentials
- Configure firewall rules

## 📚 Additional Resources

- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Node Exporter Guide](https://github.com/prometheus/node_exporter)
- [PromQL Query Language](https://prometheus.io/docs/prometheus/latest/querying/basics/)

## 🤝 Contributing

Feel free to submit issues and pull requests!

## 📝 License

MIT License - feel free to use this project for learning and production!

---

## 📊 Dashboard Preview

### Main Dashboard
- Real-time CPU and Memory graphs
- Color-coded threshold indicators
- Auto-refreshing every 5 seconds

### Alert States
- 🟢 **Inactive**: All systems normal
- 🟡 **Pending**: Threshold exceeded, waiting for duration
- 🔴 **Firing**: Alert active, action required

## 💡 Tips

- Keep an eye on the Prometheus targets page to ensure all exporters are healthy
- Use Grafana's explore feature to test PromQL queries before adding to dashboards
- Set up notification channels in Grafana for email/Slack alerts
- Regularly backup your Grafana dashboards and Prometheus data
- Monitor disk usage - Prometheus data can grow quickly!

---

**Built with ❤️ using open-source monitoring tools**