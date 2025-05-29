## ✅ **Already Done or In Progress**

- ✔️ Decided on architecture: separate mailing list form with GDPR consent, double opt-in, unsubscribe flow.
    
- ✔️ Defined required routes and behaviors.
    
- ✔️ Agreed on: soft-delete unsubscribes, secure tokens, basic bot protection, HTMX interface.
    
- ✔️ Created initial models, templates, email sender, and placeholder routes.
    

---

## 🔧 **TODO: Implementation Tasks**

### 1. **Database Layer**

- [x]  Create the `subscribers` table with the following fields:
    
    - `id`, `name`, `email`, `confirmed`, `unsubscribed`, `created_at`, `updated_at`
        
- [x]  Add an index on `email` and `confirmed` for efficiency.
    
- [x]  Set up schema migration (e.g., using `sqlx migrate`).
    

---

### 2. **Mailing List Handlers (`/api/mailing_list.rs`)**

- [ ] **`POST /api/subscribe`**
    
    - [ ]   Validate input
        
    - [ ] Reject bots via `nickname` honeypot
        
    - [ ] Create new subscriber record (or update existing one)
        
    - [ ] Send confirmation email with secure link
        
- [ ]  **`GET /api/confirm/<token>`**
    
    - [ ] Validate token (e.g., HMAC with expiry)
        
    - [ ] Mark subscriber as confirmed
        
    - [ ] Show success or expired message
        
- [ ]  **`GET /api/unsubscribe/<token>`**
    
    - [ ] Validate token
        
    - [ ] Soft-delete (set `unsubscribed = true`)
        
    - [ ] Render goodbye message
        

---

### 3. **Token Handling**

- [ ]  Implement a utility function to sign and verify tokens (e.g., `HMAC(email + timestamp)`).
    
- [ ]  Tokens should expire after a fixed window (e.g., 3–7 days).
    

---

### 4. **Email Templates**

- [ ]  Create plain-text or HTML templates for:
    
    - [ ] Confirmation email (with link to `/api/confirm/<token>`)
        
    - [ ] Optional welcome email after confirmation
        

---

### 5. **HTMX Integration**

- [ ]  Finalize `subscribe.html` with success/error handling (via `hx-target="#subscribe-message"`).
    
- [ ]  Render success message after form submit (e.g., “Check your inbox to confirm.”)
    

---

### 6. **Unsubscribe Flow**

- [ ]  Generate secure unsubscribe link included in all future mailings
    
- [ ]  Show `goodbye.html` upon visiting the unsubscribe link
    
- [ ]  Soft-delete (don’t remove from DB)
    

---

### 7. **Notifications (Plumbed for Future Use)**

- [ ]  Create `notify_subscribed()` and `notify_unsubscribed()` functions
    
- [ ]  Leave them as NO-OPs for now, but call them from relevant handlers
    

---

### 8. **Integration**

- [ ]  Mount the new routes under `/api/` in your main `App`.
    
- [ ]  Add the form to your home page.
    
- [ ]  Make sure `.env` or config contains SMTP vars:
    
    - `SMTP_USERNAME`, `SMTP_PASSWORD`, `SMTP_SERVER`
        

---

### 9. **Testing**

- [ ]  Manual test:
    
    - Submitting form
        
    - Confirm email
        
    - Unsubscribe flow
        
-  Optionally write integration tests with Actix test framework.
    

---

## 📦 Optional Future Enhancements

- Webhook support for email platform integration
    
- Admin panel to view/export subscriber list
    
- Stats (opens/clicks/unsubscribes)