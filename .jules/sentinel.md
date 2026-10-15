## 2025-02-13 - Active Session Expiration Not Enforced Dynamically
**Vulnerability:** Sessions with an `expires_at` timestamp were only checked and discarded upon initial page reload/boot. Active users could remain logged in and interact with the panel indefinitely beyond the session's official expiration time without being logged out, unless they reloaded the page.
**Learning:** Checking the session expiration during the boot sequence ensures persistent state safety but neglects dynamic active session state changes.
**Prevention:** Set up active timer-based monitors (reactive Svelte `$effect` with `setTimeout` or similar schedulers) that trigger automatic client-side logout as soon as the session's expiration time passes.

## 2025-02-14 - Client-Side Authentication Bypass via Corrupted LocalStorage Session Payload
**Vulnerability:** When restoring a persisted session from `localStorage`, the code only checked for expiration. Since `config.auth.check` is optional and defaults to true, any malformed, corrupted, or empty JSON object (e.g., `{}`) was accepted as a valid session. This allowed a complete client-side authentication bypass without a valid token or label.
**Learning:** Relying solely on external/optional revalidation hooks (`check()`) leaves the system open to session spoofing or state corruption if the raw payload retrieved from persistent storage is not strictly structure-validated first.
**Prevention:** Implement a robust runtime type-guard predicate that verifies that required session properties (such as `token` and `label` being non-empty strings) exist and have the correct types before utilizing the session or storing it.
