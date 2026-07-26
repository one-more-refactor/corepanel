## 2025-02-14 - Real-time Session Expiration on Frontend
**Vulnerability:** Weak session management where a user session could remain active indefinitely during runtime even if the `expires_at` timestamp of the session had passed, as expiration was only checked upon the initial app boot.
**Learning:** SPA frontend toolkits that persist sessions in local storage often neglect real-time, active session state monitoring during continuous user sessions.
**Prevention:** Implement a reactive background interval in the main panel component using Svelte `$effect` to check `session.expires_at` against the current epoch timestamp, triggering automated cleanup and logout on expiration.
