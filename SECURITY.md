# Security Policy

## 🔒 Security Overview

SuperIntent Telegram Bot adalah aplikasi yang menangani data user dan perform automasi penting. Dokumen ini outline security practices, policies, dan guidelines untuk ensure sistem tetap aman.

## 🛡️ Security Features

### Authentication & Authorization

#### Admin Access Control
- **Admin ID Verification** - Hanya users dengan Telegram ID di `ADMIN_IDS` dapat mengakses admin features
- **Multi-Admin Support** - Comma-separated admin IDs allow multiple admins dengan equal permissions
- **No Role Hierarchy** - Saat ini semua admin memiliki akses penuh (future: implementasi role-based access control)

```env
ADMIN_IDS=123456789,987654321  # Multiple admins
```

#### Broadcast Security
- **Preview & Confirmation** - Admin harus preview dan confirm sebelum broadcast dikirim
- **Audit Trail** - Semua broadcast operations di-log dengan timestamp dan admin ID
- **Rate Limiting** - Broadcast tidak bisa dikirim lebih dari 1x per minute (prevent spam)

### Data Protection

#### Sensitive Data Handling
- **Never Log Credentials** - Auth cookies dan tokens tidak pernah ditulis ke console/logs
- **Environment Variables** - Semua secrets stored di `.env` file (gitignore)
- **No Hardcoded Secrets** - Zero hardcoded API keys atau credentials di codebase

#### Database Security
- **Connection String** - DATABASE_URL hanya di-load dari environment variables
- **SQL Injection Prevention** - Menggunakan parameterized queries via pg library
- **User Data Minimization** - Hanya store necessary data (id, username, stats)
- **Data Retention** - User data dihapus 90 hari setelah deactivate

### API Security

#### SuperIntent API Communication
- **HTTPS Only** - Semua komunikasi dengan SuperIntent API via HTTPS dengan certificate verification
- **IPv4-Only Connections** - Force IPv4 untuk avoid DNS spoofing dan IPv6 abuse
- **Keep-Alive Agents** - HTTPS Agent dengan keep-alive reduce connection overhead dan attack surface
- **Request Timeout** - 30 second timeout prevent slow-rate attacks dan hanging connections
- **User-Agent Header** - Include proper User-Agent untuk API compatibility

#### Auth Cookie Management
- **Read-Only Cookie** - SuperIntent auth cookie dibaca dari `.env` saat startup
- **No Cookie Modification** - Bot tidak memodify atau renew cookies (handle di SuperIntent side)
- **Single User Account** - Bot menggunakan single SuperIntent account (multi-account: future feature)

### Connection Stability & Resilience

#### Exponential Backoff Retry Logic
```
Attempt 1: 2 second delay
Attempt 2: 4 second delay  
Attempt 3: 8 second delay
```

Benefits:
- **DoS Protection** - Exponential backoff reduce load pada server saat temporary outages
- **Network Resilience** - Automatic retry handle transient network failures
- **Avoid Rate Limiting** - Progressive delays respect API rate limits

#### Connection Monitoring
- **Keep-Alive HTTPS** - Persistent connections reduce new connection overhead
- **Timeout Configuration** - Individual timeouts untuk Telegram API dan SuperIntent API
- **Error Logging** - All connection errors logged untuk monitoring dan debugging

## 🚨 Threat Model & Mitigations

### Threat 1: Unauthorized Admin Access

**Risk:** Attacker gains admin access dan broadcast malicious content

**Mitigations:**
- ✅ Telegram ID verification (attacker perlu access ke admin's Telegram account)
- ✅ Preview & confirmation requirement
- ✅ Broadcast audit trail
- ✅ User dapat report abuse via Telegram channel

**Recommendations:**
- Keep ADMIN_IDS secret (don't share environment variables)
- Use strong Telegram account security (2FA recommended)
- Monitor broadcast logs regularly

### Threat 2: Database Compromise

**Risk:** Attacker gains access ke user data (Telegram IDs, stats)

**Mitigations:**
- ✅ PostgreSQL dengan encryption-at-rest (depend on database provider)
- ✅ Parameterized queries prevent SQL injection
- ✅ Limited user data stored (tidak ada email, password, atau sensitive PII)
- ✅ Database URL only in environment variables

**Recommendations:**
- Use managed PostgreSQL (Replit DB atau similar) dengan automatic backups
- Enable database monitoring dan alerts
- Regular security audits dari database access logs

### Threat 3: API Token Exposure

**Risk:** SuperIntent auth cookie leaked atau compromised

**Mitigations:**
- ✅ Auth cookie stored di environment variables (not in code)
- ✅ `.env` file gitignored (prevent accidental commits)
- ✅ No cookie transmitted over non-HTTPS connections
- ✅ Webhook-based updates (future) akan reduce long-polling dengan auth

**Recommendations:**
- Regularly rotate SuperIntent auth cookie (monthly recommended)
- Use dedicated account untuk bot (not personal account)
- Monitor API activity untuk suspicious patterns

### Threat 4: DDoS / Resource Exhaustion

**Risk:** Attacker spam API requests atau broadcast operations

**Mitigations:**
- ✅ Exponential backoff retry prevent rapid consecutive requests
- ✅ Keep-alive agents reduce connection overhead
- ✅ Broadcast rate limiting (1 per minute)
- ✅ Query timeout prevent long-running operations

**Recommendations:**
- Monitor API error rates dan implement alerts
- Implement request rate limiting per user (future feature)
- Use CDN atau load balancer jika deploy di production

### Threat 5: Broadcast Content Injection

**Risk:** Attacker modify broadcast content sebelum dikirim

**Mitigations:**
- ✅ Admin preview requirement
- ✅ No direct HTML/script execution (Telegram parses Markdown safely)
- ✅ Media type validation (hanya photo/video allowed)
- ✅ Admin confirmation prevent accidental sends

**Recommendations:**
- Regular review dari broadcast templates
- Implement content filtering untuk malicious URLs (future feature)
- Log all broadcast content untuk audit purposes

## 🔐 Security Best Practices

### For Deployment

#### Environment Variables Security
```bash
# ✅ GOOD - Secrets in environment
TELEGRAM_BOT_TOKEN=<token_here>
SUPERINTENT_AUTH_COOKIE=<cookie_here>

# ❌ BAD - Secrets in code
const TOKEN = "12345..."
```

#### Repository Security
```bash
# ✅ GOOD - .gitignore configured
.env
.env.local
node_modules/

# ❌ BAD - Secrets in git
git add .env  # DON'T DO THIS
```

#### Server Security
- **Firewall** - Block unnecessary ports, open only port used by bot
- **SSH Keys** - Use SSH keys instead of passwords untuk server access
- **Security Updates** - Keep Node.js dan dependencies updated
- **Process Isolation** - Run bot dengan limited user permissions

### For Development

#### Code Review
- Review semua code changes untuk hardcoded secrets
- Test command injection via user inputs
- Validate all API responses sebelum use

#### Testing
- Unit tests untuk auth dan authorization logic
- Integration tests dengan mock SuperIntent API
- Load testing untuk prevent resource exhaustion

#### Secrets Management
```javascript
// ✅ GOOD
const token = process.env.TELEGRAM_BOT_TOKEN;

// ❌ BAD
const token = "12345..."; // Don't hardcode!
```

## 🐛 Reporting Security Vulnerabilities

Jika anda menemukan security vulnerability, **JANGAN** report di public issue. Sebagai gantinya:

1. **Email us** - security@superintent.local (jika available)
2. **Telegram DM** - Contact admin di private message
3. **Include Details:**
   - Description dari vulnerability
   - Steps untuk reproduce
   - Potential impact
   - Suggested fix (optional)

### Response Timeline
- **Acknowledgment** - Within 24 hours
- **Initial Assessment** - Within 72 hours
- **Fix & Release** - Depend on severity (critical: ASAP, low: next release)

### Vulnerability Severity

| Level | Description | Example |
|-------|-------------|---------|
| **Critical** | Immediate impact, widespread usage affected | Hardcoded secrets, authentication bypass |
| **High** | Significant security impact | SQL injection, unauthorized data access |
| **Medium** | Moderate impact, some mitigation exists | Unvalidated input, weak error handling |
| **Low** | Minor impact atau difficult to exploit | Information disclosure, non-critical bugs |

## 📋 Security Checklist

### Before Deployment
- [ ] All secrets in `.env` file (not in code)
- [ ] `.env` file in `.gitignore`
- [ ] ADMIN_IDS configured dengan correct Telegram IDs
- [ ] Database credentials secured
- [ ] HTTPS enabled untuk semua external communications
- [ ] Error logging tidak expose sensitive information
- [ ] Dependencies updated ke latest stable versions

### During Operation
- [ ] Monitor logs untuk suspicious activities
- [ ] Review broadcast history regularly
- [ ] Check database connection status
- [ ] Verify backup procedures berjalan
- [ ] Monitor admin access patterns
- [ ] Track API errors dan failures

### Maintenance
- [ ] Update dependencies monthly
- [ ] Rotate auth cookies quarterly
- [ ] Review security logs bi-weekly
- [ ] Test disaster recovery procedures
- [ ] Update security documentation

## 🔄 Security Updates & Patches

### Dependency Updates
```bash
# Check untuk vulnerable packages
npm audit

# Update packages (safe)
npm update

# Update packages (may break compatibility)
npm audit fix
```

### Bot Updates
- Follow release notes untuk security patches
- Test updates di staging sebelum production
- Keep backup dari current working version

## 📚 References

- **Telegram Bot Security** - https://core.telegram.org/bots/faq#how-secure-are-bots
- **Node.js Security** - https://nodejs.org/en/docs/guides/security/
- **PostgreSQL Security** - https://www.postgresql.org/docs/current/sql-syntax.html
- **OWASP Top 10** - https://owasp.org/www-project-top-ten/

## ❓ FAQ

**Q: Apakah bot bisa di-hack?**  
A: Seperti semua software, ada security risks. Kami implement multiple layers dari security controls, tapi tidak ada sistem yang 100% aman. Report vulnerabilities responsibly.

**Q: Bagaimana user data diproteksi?**  
A: User data disimpan di encrypted PostgreSQL database, transmitted via HTTPS, dan tidak pernah shared dengan third parties.

**Q: Apakah auth cookie aman?**  
A: Ya, if kamu keep `.env` file secret dan tidak share credentials. Ganti cookie regularly untuk extra security.

**Q: Apa yang terjadi jika admin account compromised?**  
A: Attacker bisa broadcast messages ke semua users. Immediately rotate ADMIN_IDS dan notify users tentang situation.

**Q: Apakah bot store passwords?**  
A: Tidak. Bot hanya store Telegram IDs dan user stats. Passwords disimpan di SuperIntent (not bot's responsibility).

---

**Last Updated:** December 2025  
**Version:** 1.0  
**Maintained By:** Hokireceh Team
