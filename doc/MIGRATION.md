# Migration

## From 0.1.x to 0.2.0

### Vault Encryption

The key derivation for vault file encryption has been upgraded to
`PKCS5_PBKDF2_HMAC` (SHA-256, random 16-byte salt, 600,000 iterations).

This is a **breaking change**. Existing vault files encrypted with the
old method cannot be decrypted by version 0.2.0.

**Action required:**

1. Stop pgmoneta-mcp
2. Delete the existing admin configuration file:
   - `pgmoneta_admins.conf` (or the file specified with `-f`)
3. Delete the existing master key:
   - On Linux/Unix: `rm ~/.pgmoneta/master.key`
4. Regenerate the master key:
   ```
   pgmoneta-mcp-admin master-key
   ```
5. Re-add all users/admins:
   ```
   pgmoneta-mcp-admin user add -U <username> -P <password> -f <admins_file>
   ```
6. Restart pgmoneta-mcp
