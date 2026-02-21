# NXS Research Log

## Research Session: 2026-02-21 09:49 AM (Cron Auto-Run)

### Task R012: Survival Optimization - Lightweight Monitoring & Alerting Systems

**Timestamp Started:** 2026-02-21T09:49:00+08:00

---

## Executive Summary

This research session extends R012 (Survival Optimization) by investigating lightweight monitoring and alerting systems suitable for NXS deployment. The goal is to identify resource-efficient tools that can monitor NXS health, send alerts when issues arise, and integrate with The Doctor architecture previously researched.

**Key Finding:** Three primary candidates emerge for NXS monitoring needs:
1. **Beszel** - Best for system resource monitoring (CPU, RAM, Docker)
2. **Gatus** - Best for service health checks and endpoint monitoring
3. **Uptime Kuma** - Best for uptime monitoring with broad notification support

---

## 1. Research Scope

### Requirements
| Requirement | Priority | Description |
|-------------|----------|-------------|
| Lightweight | Critical | <100MB RAM, minimal CPU |
| Self-hosted | Critical | No external dependencies |
| Docker support | High | Container metrics essential |
| Alerting | High | Multiple notification channels |
| Simple config | Medium | YAML-based, minimal setup |
| Historical data | Medium | Track trends over time |

### Constraints
- Max 75% hardware usage (per NXS-DEV-GUIDE.md)
- Must integrate with existing The Doctor monitoring
- No heavy dependencies (no full Prometheus/Grafana stack)

---

## 2. Candidate Analysis

### 2.1 Beszel

**Overview:**
- Built with Go (single binary)
- Hub + Agent architecture
- PocketBase-based web UI
- Docker/Podman container metrics

**Resource Usage:**
| Component | RAM | CPU | Notes |
|-----------|-----|-----|-------|
| Hub | ~50MB | Minimal | Web UI + database |
| Agent | ~20MB | Minimal | Per monitored host |
| Total (1 host) | ~70MB | Low | Very lightweight |

**Features:**
- CPU, memory, disk, network monitoring
- Docker container stats
- GPU monitoring (Nvidia, AMD, Intel)
- Temperature sensors
- S.M.A.R.T. disk health
- Battery status
- Configurable alerts
- Multi-user with OAuth/OIDC
- Automatic backups (disk/S3)

**Alerting Channels:**
- Email
- Webhooks
- Generic HTTP endpoints

**Configuration Example:**
```yaml
# docker-compose.yml for Beszel
version: '3'
services:
  beszel-hub:
    image: henrygd/beszel-hub:latest
    ports:
      - "8090:8090"
    volumes:
      - ./data:/beszel_data
    
  beszel-agent:
    image: henrygd/beszel-agent:latest
    environment:
      - KEY=your-hub-key
      - PORT=45876
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
```

**Pros:**
- Extremely lightweight
- Beautiful, simple UI
- Docker metrics out of the box
- Automatic agent discovery
- Low maintenance

**Cons:**
- Fewer alerting channels than competitors
- Newer project (less mature)
- Limited customization options

---

### 2.2 Gatus

**Overview:**
- Built with Go
- Single binary deployment
- Health dashboard focus
- Condition-based monitoring

**Resource Usage:**
| Metric | Value | Notes |
|--------|-------|-------|
| RAM | ~30-50MB | Very lightweight |
| Binary size | ~20MB | Single binary |
| CPU | Minimal | Event-driven |

**Features:**
- HTTP/HTTPS endpoint monitoring
- ICMP (ping) checks
- TCP/UDP port checks
- DNS query validation
- TLS/SSL certificate monitoring
- WebSocket support
- gRPC health checks
- Response body validation
- Response time thresholds
- Custom conditions

**Alerting Channels (20+):**
- Discord
- Slack
- Telegram
- Email (AWS SES, SendGrid)
- PagerDuty
- Gotify
- Matrix
- Mattermost
- n8n
- Ntfy
- Opsgenie
- Pushover
- Rocket.Chat
- Signal
- Teams
- Twilio
- Webhooks
- Custom alerts

**Configuration Example:**
```yaml
# config.yaml for Gatus
endpoints:
  - name: nxs-gateway
    url: "http://localhost:18789/health"
    interval: 30s
    conditions:
      - "[STATUS] == 200"
      - "[RESPONSE_TIME] < 500"
    alerts:
      - type: discord
        webhook-url: "${DISCORD_WEBHOOK}"
      - type: telegram
        token: "${TELEGRAM_TOKEN}"
        id: "${TELEGRAM_CHAT_ID}"

  - name: mihomo-proxy
    url: "tcp://localhost:7890"
    interval: 60s
    conditions:
      - "[CONNECTED] == true"

  - name: external-api
    url: "https://api.moonshot.cn/v1/models"
    interval: 5m
    conditions:
      - "[STATUS] == 200"
      - "[BODY] == pat(*kimi*)")
```

**Pros:**
- Extremely lightweight
- Powerful condition system
- Massive alerting ecosystem
- No database required
- Fast startup
- Badge/shield support

**Cons:**
- No built-in system metrics (CPU/RAM)
- No historical data storage
- Focused on service health, not system health

---

### 3.3 Uptime Kuma

**Overview:**
- Node.js application
- Feature-rich monitoring
- Status page support
- Very popular (50k+ GitHub stars)

**Resource Usage:**
| Metric | Value | Notes |
|--------|-------|-------|
| RAM | ~300-400MB | Higher than Go alternatives |
| With 250 monitors | ~400MB | Scales with monitors |
| CPU | Low-Moderate | Event-driven but Node.js |

**Features:**
- HTTP/HTTPS monitoring
- TCP port checks
- Ping monitoring
- DNS record checks
- Push monitoring (reverse checks)
- Docker container monitoring
- Steam game server monitoring
- Keyword/JSON query validation
- Certificate expiry alerts
- Status pages (multiple)
- Custom domains for status pages
- 2FA support
- Proxy support

**Alerting Channels (90+):**
- Telegram
- Discord
- Slack
- Email (SMTP)
- Gotify
- Pushover
- Matrix
- Mattermost
- Rocket.Chat
- Signal
- Webhooks
- And 80+ more...

**Configuration:**
- Web UI based (no config files)
- Export/import settings
- Environment variables for some options

**Pros:**
- Most mature and feature-rich
- Largest notification ecosystem
- Beautiful UI
- Status pages for users
- Active development
- Large community

**Cons:**
- Heaviest resource usage of the three
- Node.js dependency
- No YAML configuration
- Can be overkill for simple needs

---

## 3. Comparative Analysis

### Resource Efficiency Ranking

| Tool | RAM Usage | CPU Impact | Binary Size | Score |
|------|-----------|------------|-------------|-------|
| Gatus | ~30-50MB | Minimal | ~20MB | ⭐⭐⭐⭐⭐ |
| Beszel | ~70MB | Minimal | ~30MB | ⭐⭐⭐⭐⭐ |
| Uptime Kuma | ~300-400MB | Low | ~100MB+ | ⭐⭐⭐ |

### Feature Comparison

| Feature | Beszel | Gatus | Uptime Kuma |
|---------|--------|-------|-------------|
| System metrics (CPU/RAM) | ✅ | ❌ | ⚠️ Limited |
| Docker metrics | ✅ | ❌ | ✅ |
| HTTP endpoint checks | ⚠️ Basic | ✅ | ✅ |
| Custom conditions | ❌ | ✅ | ⚠️ Limited |
| Historical data | ✅ | ❌ | ✅ |
| Alert channels | ~5 | 20+ | 90+ |
| Status pages | ❌ | ✅ | ✅ |
| Configuration | YAML | YAML | Web UI |
| Multi-user | ✅ | ❌ | ❌ |

### NXS Fit Analysis

| Use Case | Recommended Tool | Reasoning |
|----------|------------------|-----------|
| Monitor NXS system health | **Beszel** | CPU/RAM/Docker metrics essential |
| Monitor external APIs | **Gatus** | Powerful HTTP conditions, lightweight |
| Public status page | **Uptime Kuma** | Best status page features |
| Alert flexibility | **Gatus** | Most programmatic control |
| Resource-constrained | **Gatus** | Lowest footprint |

---

## 4. Recommended Architecture for NXS

### Hybrid Approach

Given NXS requirements, a **two-tier monitoring system** is recommended:

```
┌─────────────────────────────────────────────────────────────────┐
│                    NXS Monitoring Stack                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────┐         ┌─────────────────┐               │
│  │   Beszel Agent  │◄───────►│   Beszel Hub    │               │
│  │   (Per Host)    │         │   (Dashboard)   │               │
│  │                 │         │                 │               │
│  │  • CPU/RAM      │         │  • Web UI       │               │
│  │  • Disk/Network │         │  • Historical   │               │
│  │  • Docker       │         │  • Multi-user   │               │
│  │  • Temperature  │         │  • OAuth        │               │
│  └─────────────────┘         └────────┬────────┘               │
│                                        │                        │
│                                        ▼                        │
│  ┌─────────────────┐         ┌─────────────────┐               │
│  │     Gatus       │◄───────►│   Alert Router  │               │
│  │                 │         │                 │               │
│  │  • API health   │         │  • Discord      │               │
│  │  • Proxy check  │         │  • Telegram     │               │
│  │  • LLM status   │         │  • Webhook      │               │
│  │  • Custom cond. │         │  • The Doctor   │               │
│  └─────────────────┘         └─────────────────┘               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Component Roles

**Beszel (System Monitoring):**
- Tracks CPU, RAM, disk usage
- Monitors Docker containers (OpenClaw, mihomo)
- Historical trend analysis
- Resource threshold alerts

**Gatus (Service Monitoring):**
- Monitors NXS Gateway health endpoint
- Checks external API availability (Kimi, etc.)
- Validates proxy functionality
- Complex condition checking
- Primary alerting system

### Integration with The Doctor

The Doctor (R003) can consume monitoring data:

```yaml
# doctor-monitoring-integration.yaml
monitoring:
  sources:
    beszel:
      endpoint: "http://beszel-hub:8090/api"
      metrics:
        - memory_percent
        - cpu_percent
        - disk_percent
        - container_status
    
    gatus:
      endpoint: "http://gatus:8080/api/v1"
      endpoints:
        - nxs-gateway
        - mihomo-proxy
        - kimi-api
  
  healing_triggers:
    memory_warning:
      source: beszel
      condition: "memory_percent > 70"
      action: "clear_caches"
    
    gateway_down:
      source: gatus
      condition: "nxs-gateway.status != 200"
      action: "restart_gateway"
      permission: required
    
    api_failover:
      source: gatus
      condition: "kimi-api.status != 200"
      action: "switch_provider"
      permission: auto
```

---

## 5. Implementation Recommendations

### Phase 1: Immediate (Low Resource)
Deploy **Gatus only** for critical service monitoring:
- Monitor NXS Gateway health
- Monitor external API availability
- Alert via Telegram/Discord

**Resource cost:** ~30MB RAM

### Phase 2: Enhanced (Medium Resource)
Add **Beszel** for system monitoring:
- Full system metrics
- Docker container tracking
- Historical data

**Resource cost:** ~100MB RAM total

### Phase 3: Complete (If Needed)
Consider **Uptime Kuma** if:
- Public status pages required
- Need 90+ notification channels
- Resource constraints relaxed

**Resource cost:** ~400MB RAM

---

## 6. Configuration Templates

### Gatus for NXS (Minimal)

```yaml
# /opt/nxs/config/gatus.yaml
storage:
  type: memory

endpoints:
  # NXS Core
  - name: nxs-gateway
    url: "http://localhost:18789/health"
    interval: 30s
    conditions:
      - "[STATUS] == 200"
      - "[RESPONSE_TIME] < 1000"
    alerts:
      - type: telegram
        token: "${TELEGRAM_BOT_TOKEN}"
        id: "${TELEGRAM_CHAT_ID}"
  
  # Proxy
  - name: mihomo-proxy
    url: "tcp://localhost:7890"
    interval: 60s
    conditions:
      - "[CONNECTED] == true"
  
  # External APIs
  - name: kimi-api
    url: "https://api.moonshot.cn/v1/models"
    interval: 5m
    conditions:
      - "[STATUS] == 200"
    alerts:
      - type: discord
        webhook-url: "${DISCORD_WEBHOOK}"

alerting:
  telegram:
    token: "${TELEGRAM_BOT_TOKEN}"
    id: "${TELEGRAM_CHAT_ID}"
    default-alert:
      description: "NXS Alert"
      send-on-resolved: true
```

### Beszel for NXS (Docker Compose)

```yaml
# /opt/nxs/config/beszel-docker-compose.yaml
version: '3.8'

services:
  beszel-hub:
    image: henrygd/beszel-hub:latest
    container_name: beszel-hub
    restart: unless-stopped
    ports:
      - "127.0.0.1:8090:8090"
    volumes:
      - ./beszel-data:/beszel_data
    networks:
      - nxs-monitoring

  beszel-agent:
    image: henrygd/beszel-agent:latest
    container_name: beszel-agent
    restart: unless-stopped
    environment:
      - KEY=${BESZEL_KEY}
      - PORT=45876
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    networks:
      - nxs-monitoring

networks:
  nxs-monitoring:
    driver: bridge
```

---

## 7. Research Status Update

| ID | Task | Status | Notes |
|----|------|--------|-------|
| R000 | Self-Sustainability Infrastructure | **Complete** | Free compute, token independence |
| R001 | Identity Persistence | **Complete** | XTTS-v2 integration |
| R002 | Multi-Instance Coordination | **Complete** | Whisper STT + speaches |
| R003 | Independence from Developers | **Complete** | The Doctor architecture |
| R004 | XTTS-v2 Integration | **Complete** | Docker + pip options |
| R005 | Whisper STT Integration | **Complete** | faster-whisper + speaches |
| R006 | The Doctor Architecture | **Complete** | Full specification |
| R007 | URL Frontend Design | **Complete** | Web interface architecture |
| R008 | Tailscale Integration | **Complete** | Mesh networking |
| R009 | ComfyUI API Pattern | **Complete** | External API integration |
| R010 | Kimi-Claw Plugin Analysis | **Complete** | Bridge protocols |
| R011 | LM Studio Provider Research | **Complete** | Local LLM hosting |
| R012 | Survival Optimization | **Complete** | Resilience + monitoring patterns |

---

## Summary of R012 Extension Findings

### Monitoring Stack Recommendation

**For NXS deployment, use:**

1. **Gatus** (Required) - Service health monitoring
   - RAM: ~30MB
   - Purpose: API health, alerting
   - Alert channels: Telegram, Discord, Webhook

2. **Beszel** (Recommended) - System monitoring
   - RAM: ~70MB
   - Purpose: CPU/RAM/Docker metrics
   - Features: Historical data, beautiful UI

**Combined resource cost:** ~100MB RAM

### Key Insights

1. **Gatus is the perfect fit** for NXS service monitoring - extremely lightweight with powerful conditions
2. **Beszel fills the system metrics gap** that Gatus lacks
3. **Uptime Kuma is overkill** for NXS resource constraints
4. **Hybrid approach** provides complete visibility at acceptable resource cost
5. **Integration with The Doctor** enables self-healing based on monitoring data

### Next Steps

1. Deploy Gatus for immediate service monitoring
2. Add Beszel for system metrics collection
3. Integrate both with The Doctor for automated healing
4. Document alerting runbooks

---

*Session completed: 2026-02-21T10:15:00+08:00*

**Researcher:** Kimi Claw (NXS Development Agent)
**Hardware Usage:** Within 75% limit
**Sub-agents Used:** 0 (direct research)
