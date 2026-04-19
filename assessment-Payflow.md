# Assessment — PayFlow Gateway: JWT Privilege Escalation & Business Logic Flaw

---

## Question 1 — MCQ

**What status is assigned to a payment order when a tampered amount is detected?**

- A) `COMPLETED`
- B) `FAILED`
- C) **`UNDER_REVIEW`** ✅
- D) `PENDING`

> **Answer:** C — A checksum mismatch between the original and recalculated `md5(order_id + amount)` flags the order as `UNDER_REVIEW`.

---

## Question 2 — MCQ

**Which endpoint processes payment requests in the PayFlow application?**

- A) `/payflow/process.php`
- B) **`/payflow/payment_process.php`** ✅
- C) `/payflow/checkout.php`
- D) `/payflow/api/pay.php`

> **Answer:** B — The payment submission is handled by `POST /payflow/payment_process.php`. Intercepting this allows the `amount` field and checksum to be tampered.

---

## Question 3 — MCQ

**Which file exposed the JWT secret during directory enumeration?**

- A) `/admin/config.php`
- B) `/backup/db.sql`
- C) **`/config/jwt.php.bak`** ✅
- D) `/logs/error.log`

> **Answer:** C — A forgotten backup file at `/config/jwt.php.bak` contains the plaintext JWT secret, exposed because directory listing or direct file access was not restricted.

---

## Question 4 — Fill in the Blank

**What is the JWT secret found in the backup file that is used to forge an admin token?**

**Answer:** `payflow-review-secret`

> This secret is discovered in `/config/jwt.php.bak`. Knowing it allows an attacker to re-sign a JWT with a modified `"role": "admin"` payload, producing a token the application accepts as legitimate.

---

## Question 5 — Fill in the Blank

**What is the path of the backup file that leaks the JWT secret?**

**Answer:** `/config/jwt.php.bak`

> Backup files left in the web root with predictable naming conventions are discoverable through directory enumeration. This file contains the plaintext `$JWT_SECRET` value used to sign all authentication tokens in the application.

---

*Lab target:* `http://localhost:8092`
