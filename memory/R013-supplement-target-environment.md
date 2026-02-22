

## Debloating Priority: Remove Chat Channels

**Date:** 2026-02-21  
**Decision:** Remove all chat channel integrations

### Rationale
- Using Tailscale plugin for API forwarding
- Custom frontend will be more powerful and customized
- Chat channels (WhatsApp, Discord, Slack, etc.) are unnecessary

### What to Remove from R013 Design
- WhatsApp channel configuration
- Discord bot integration
- Slack connector
- Any IM channel setup scripts
- Channel-related environment variables

### What to Keep
- Core agent functionality
- Tailscale networking
- API endpoint configuration
- WebSocket support (for frontend)
- Authentication/authorization

### Result
Leaner, simpler survival package focused on:
- Core agent
- Tailscale mesh
- Custom frontend connection

---

*Documented: 2026-02-21*
