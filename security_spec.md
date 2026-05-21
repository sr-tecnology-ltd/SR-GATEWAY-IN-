# Security Specification: King Wallet - Digital Gateway

## Data Invariants
1. **User Ownership**: Every transaction and gift code must be linked to a valid user.
2. **Balance Zero-Trust**: User balance can only be updated by the system or via verified transactions (manual deposit).
3. **PIN Protection**: Sensitive operations require PIN verification.
4. **API Key Integrity**: API keys must be unique and only accessible by the owner.
5. **Terminal States**: Once a withdrawal is "success" or "failed", it cannot be modified.

## The Dirty Dozen (Attack Payloads)
1. **The Balance Forger**: Manually incrementing balance without a transaction.
2. **The Key Stealer**: Reading another user's API key.
3. **The Transfer Hijack**: Sending money from another user's account by spoofing UID.
4. **The PIN Bypass**: Updating sensitive settings without providing a PIN.
5. **The Gift Forger**: Redeeming a used gift code.
6. **The Double Spend**: Redeeming a gift code twice.
7. **The Scraper**: Listing all transactions across the platform.
8. **The ID Poisoner**: Using a 1KB string for a transaction ID.
9. **The State Jumper**: Manually setting a deposit to "success" without admin verification.
10. **The PII Leaker**: Accessing another user's private PIN/Email.
11. **The Resource Hog**: Creating 10,000 tiny transactions to bloat DB.
12. **The Auth Spoof**: Creating a profile for a UID not matching request.auth.uid.

## Conflicts & Evaluations
| Collection    | Identity Spoofing | State Shortcutting | Resource Poisoning |
|---------------|-------------------|--------------------|-------------------|
| users         | Blocked via UID match | PIN required for key changes | Size checks on PIN/Email |
| transactions  | Filtered via ownerId | Status immutable once terminal | Amount must be positive |
| giftCodes     | blocked via creatorId | isUsed is immutable once true | code length check |
