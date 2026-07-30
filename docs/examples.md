# mcp-notify examples

Real-time monitoring and notifications for the MCP Registry ecosystem.

## Example 1

```text
┌────────────────────────────────────────────────────────────────┐
│                        MCP Notify                              │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌──────────┐    ┌──────────┐    ┌──────────────────────────┐  │
│  │  Poller  │───▶│  Differ  │───▶│  Notification Dispatcher │  │
│  └──────────┘    └──────────┘    └──────────────────────────┘  │
│       │               │                      │                 │
│       ▼               ▼                      ▼                 │
│  ┌──────────┐    ┌──────────┐    ┌──────────────────────────┐  │
│  │ Registry │    │ Snapshot │    │        Channels          │  │
│  │   API    │    │  Store   │    │  ┌───────┐ ┌───────────┐ │  │
│  └──────────┘    └──────────┘    │  │Discord│ │   Slack   │ │  │
│                       │          │  └───────┘ └───────────┘ │  │
│                       ▼          │  ┌───────┐ ┌───────────┐ │  │
│                  ┌──────────┐    │  │ Email │ │  Webhook  │ │  │
│                  │PostgreSQL│    │  └───────┘ └───────────┘ │  │
│                  └──────────┘    │  ┌───────┐               │  │
│                                  │  │  RSS  │               │  │
│                                  │  └───────┘               │  │
│                                  └──────────────────────────┘  │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                       REST API                           │  │
│  │  /subscriptions  /changes  /feeds  /health  /metrics     │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    Web Dashboard                         │  │
│  │  React + TypeScript + Tailwind + shadcn/ui               │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

## Example 2

```bash
# Clone the repository
git clone https://github.com/nirholas/mcp-notify.git
cd mcp-notify

# Configure environment
cp .env.example .env
# Edit .env with your settings

# Start services
docker compose up -d

# Access dashboard at http://localhost:8080
```

## Example 3

```bash
# Create a webhook subscription via API
curl -X POST http://localhost:8080/api/v1/subscriptions \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My DeFi Alerts",
    "filters": {
      "keywords": ["defi", "ethereum", "swap"],
      "namespaces": ["io.github.*"]
    },
    "channels": [{
      "type": "discord",
      "config": {
        "webhook_url": "https://discord.com/api/webhooks/..."
      }
    }]
  }'
```

## Example 4

```bash
# Install CLI
go install github.com/nirholas/mcp-notify/cmd/mcp-notify-cli@latest

# Check for recent changes
mcp-notify-cli changes --since 24h

# Watch with live output
mcp-notify-cli watch --filter "defi,blockchain" --output json

# Subscribe to notifications
mcp-notify-cli subscribe \
  --discord-webhook "https://discord.com/api/webhooks/..." \
  --filter "io.github.myorg/*"
```

## Example 5

```bash
git clone https://github.com/nirholas/mcp-notify.git
cd mcp-notify
make build
./bin/mcp-notify --config config.yaml
```

## Example 6

```bash
docker pull ghcr.io/nirholas/mcp-notify:latest
docker run -p 8080:8080 -v $(pwd)/config.yaml:/app/config.yaml \
  ghcr.io/nirholas/mcp-notify:latest
```

## Example 7

```bash
helm repo add mcp-notify https://YOUR_USERNAME.github.io/mcp-notify
helm install mcp-notify mcp-notify/mcp-notify \
  --set config.registryUrl=https://registry.modelcontextprotocol.io \
  --set notifications.discord.enabled=true
```

## Example 8

```bash
# Create subscription
POST /api/v1/subscriptions

# List subscriptions
GET /api/v1/subscriptions

# Get subscription
GET /api/v1/subscriptions/{id}

# Update subscription
PUT /api/v1/subscriptions/{id}

# Delete subscription
DELETE /api/v1/subscriptions/{id}

# Pause/resume subscription
POST /api/v1/subscriptions/{id}/pause
POST /api/v1/subscriptions/{id}/resume
```


Every snippet above is taken from the [repository documentation](https://github.com/nirholas/mcp-notify#readme).
