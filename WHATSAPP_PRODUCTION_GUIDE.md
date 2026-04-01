# WhatsApp + Claude Production Migration Guide

Step-by-step checklist to move from Twilio sandbox to a dedicated production WhatsApp Business number.

---

## Prerequisites

- [ ] **Twilio account** — Upgraded (payment method linked)  
  [Sign up / Upgrade](https://www.twilio.com/try-twilio) → Admin → Account billing → Upgrade account

- [ ] **Meta Business Portfolio** — You need one of:
  - Existing Meta Business Portfolio with **admin access (full permissions)**
  - Or create one during Self Sign-up → then complete **Meta Business Verification** before production

- [ ] **Phone number** — Must meet:
  - Can receive SMS or voice calls (for OTP verification)
  - Not registered with WhatsApp or WhatsApp Business app (delete account first if needed)
  - Not banned on WhatsApp
  - Options: Twilio number (~$1–2/mo) or your own number (BYON)

---

## Part 1: Meta Business Verification (if new portfolio)

Required if you created a new Meta Business Portfolio during sign-up. Without it, you stay limited.

**Documents needed (2–3 of):**
- Business license  
- Certificate of incorporation  
- Utility bill (to verify name, address, phone)

**Where:** [Meta Business Manager](https://business.facebook.com/settings/security) → Business verification

**Timeline:** 10 minutes to 14 working days

---

## Part 2: Register Your Production Number (Twilio Self Sign-up)

### Step 1: Go to WhatsApp Senders
Twilio Console → **Messaging** → **Senders** → **WhatsApp Senders**  
Or: [console.twilio.com → WhatsApp Senders](https://console.twilio.com/us1/develop/sms/senders/whatsapp-senders)

### Step 2: Start Self Sign-up
- Click **Create new sender**
- Select a phone number (Twilio or non-Twilio)
- Click **Continue**

### Step 3: Link WhatsApp Business Account
- Click **Continue with Facebook**
- Complete steps **in the same browser** (keep pop-up and Console open)

### Step 4: Meta flow
1. **Log in to Facebook** → Allow Twilio to manage your WABA  
2. **Business info** → Create new Meta Business Portfolio or select existing  
3. **WABA** → Create new WhatsApp Business Account (don’t use one from another provider)  
4. **Business profile:**
   - Display name — must follow [Meta’s guidelines](https://www.facebook.com/business/help/757569725593362)
   - Category — e.g. Business & Utility
   - Optional: description, website

### Step 5: Add & Verify Phone Number
- **Twilio number:** Copy verification code from Twilio Console (SMS) or email (voice)
- **Non-Twilio:** Get code via SMS or voice on that number
- Paste/enter code in Meta pop-up → **Confirm**

### Step 6: Finish
- Review Twilio’s access request → **Confirm**
- Wait a few minutes for registration
- Sender status should become **ONLINE**

---

## Part 3: Configure Production Webhooks

1. Click your WhatsApp sender → **Edit Sender**  
2. **Inbound Settings**:
   - **When a message comes in:** `https://your-domain.com/api/whatsapp/webhook`
   - Method: **POST**  
3. **Status callback URL** (optional): `https://your-domain.com/api/whatsapp/status`

Your webhook must:
- Be **HTTPS** (no localhost in production)
- Verify Twilio signatures (see [Verify Webhook Signatures](https://www.twilio.com/docs/usage/security#validating-requests))
- Parse `Body`, `From`, `To` from the POST body
- Respond within ~15 seconds (use async processing for Claude calls if needed)

---

## Part 4: Message Templates (First Contact)

WhatsApp requires approved **message templates** for the *first* message to a user in any 24‑hour window.

- **Within 24h of user reply:** You can send freeform text.
- **Outside 24h window:** You must use an approved template.

**Create templates:** Twilio Console → Messaging → Content Template Builder  
Or use [Meta’s template format](https://developers.facebook.com/docs/whatsapp/cloud-api/guides/send-message-templates).

Example starter templates:
- `hello_world` — “Hello {{1}}, welcome to [Your Bot]!”
- `welcome` — “Hi! How can I help you today?”

---

## Part 5: Environment Variables (Production)

```env
# Twilio
TWILIO_ACCOUNT_SID=ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_WHATSAPP_NUMBER=whatsapp:+14155551234

# Anthropic
ANTHROPIC_API_KEY=sk-ant-xxxxxxxx

# Webhook validation (optional but recommended)
TWILIO_WEBHOOK_SECRET=your_webhook_auth_token
```

Store secrets in your hosting provider (Vercel, Railway, etc.), never in code.

---

## Part 6: Production Checklist

| Item | Status |
|------|--------|
| Twilio account upgraded | ⬜ |
| Meta Business Portfolio (admin access) | ⬜ |
| Meta Business Verification complete | ⬜ |
| Phone number acquired & verified | ⬜ |
| WhatsApp sender status = ONLINE | ⬜ |
| Webhook URL set (HTTPS) | ⬜ |
| Webhook signature validation enabled | ⬜ |
| At least one message template approved | ⬜ |
| Claude API key in env | ⬜ |
| Error logging & monitoring set up | ⬜ |

---

## Costs (Approximate)

| Item | Cost |
|------|------|
| Twilio phone number | ~$1–2/month |
| WhatsApp conversation (per 24h) | Per Meta’s pricing (utility, marketing, service tiers) |
| Claude API | Per token |
| Hosting (Vercel/Railway) | Free tier or ~$5/month |

---

## Troubleshooting

**Sender not ONLINE**  
- Check Meta Business verification status  
- Ensure number can receive SMS/voice

**Webhook not receiving messages**  
- Confirm URL is HTTPS and reachable  
- Check Twilio debugger for errors  
- Verify webhook path matches exactly

**Can’t message users first**  
- Use an approved template for first contact  
- Or wait for user to message you (within 24h you can reply freely)

**Number already on WhatsApp**  
- If on WhatsApp/WhatsApp Business app: [delete the account](https://faq.whatsapp.com/2138577903196467)  
- If on another Business Platform: turn off 2FA in WhatsApp Manager, then migrate

---

## Next Steps After Production

- [ ] Add message template for cold outreach (if needed)
- [ ] Set up usage/rate limits to control costs
- [ ] Add analytics (messages, latency, errors)
- [ ] Apply for [WhatsApp Official Business Account](https://www.facebook.com/business/help/604726921052590) (optional; green checkmark)
