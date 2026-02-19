# HEARTBEAT.md - The Pulse of Operations

# Use this file to queue periodic checks.
# If empty, the agent replies HEARTBEAT_OK.

# --- DAILY ROUTINE ---

[06:00] CHECK: Containers (N8N, Evo), Backups, Disk Space. Report status.
# [12:00] CHECK: Inbox (Email/WhatsApp) for new leads. API Latency.
# [18:00] CHECK: Uptime summary. Security logs (Fail2Ban).
# [00:00] CHECK: Cleanup logs. Verify database integrity.

# --- ACTIVE TASKS ---
# - Monitor Porto Prospecção Campaign (STOPPED BY USER)
