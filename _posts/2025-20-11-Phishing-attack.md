---
date: "2025-11-24T00:00:00Z"
title: What is Phishing (PART 1)
tags : ["Phishing"]
categories: ["Security Awareness"]

---
## What is Phishing?
Phishing is defined as a cybercrime in which attackers pose as trusted entities, such as a bank, a colleague, HR, or a delivery service, to trick victims into revealing sensitive information or installing malware by opening attached files. In this blog, I'll be covering spearphishing and show a sample of a spearphishing email.

### Spearphishing
Spearphishing is a highly targeted form of phishing cyber attack. Unlike standard phishing, which sends generic emails to thousands of people, **Spearphishing** targets a specific individual, organization, or department.

**Why it Works So Well**

1. **High Personalization:** Attackers do their research about you on social media platforms like LinkedIn and Facebook, the company websites, among others, to see who your boss is, identify your role and department, or learn what projects you are currently working on.
2.  **Established Trust:** With this acquired information, they craft emails with real, recognizable details that you are much more likely to trust these references and lower your doubts and defenses.
3.  **The "Malicious Document" Connection:** In sophisticated attacks, hackers often use spearphishing to deliver their initial payload embedded in a PDF, Excel, or Word document. These files are designed to appear legitimate, for example, by seeming to originate from your internal Finance Department.

***

### Sample Spearphishing Email

> **From:** Accounts Payable `(notifications@ITTYK.com)`
>
> **To:** `clarajames.2@TTYK.com`
>
> **Date:** November 20, 2025, 8:45 AM
>
> **Subject:** **URGENT: Overdue Invoice Payment (#INV-2025-992)**
>
> Dear Clara James,
>
> We have detected a discrepancy in the Q3 vendor payments allocated to your department. Invoice **#INV-2025-992** for **$14,250.00** is currently 30 days overdue.
>
> Please review the attached remittance advice immediately to confirm the services rendered so we can release the payment hold.
>
> **SECURITY NOTICE:**
>
>  Because this document contains sensitive banking details, it is password-protected.
> **Password:** `SecurePay2025`
>
> Regards,
>
> **Sarah Jenkins**
>
> Senior Financial Controller
>
> ---
> 📎 **Attachment:** `Invoice_INV-992_Details.docm` (42 KB)

***

### Why This is a Phishing email

At first glance, this email appears to be a standard business communication. However, a closer look reveals the specific **red flags** that identify it as a malicious spearphishing attempt;

**1. The "From" Address Mismatch**
* The display name says **"Accounts Payable,"** implying it is an internal department from TTYK. However, the actual sending address is `notifications@ITTYK.com`. Legitimate internal emails should come from your company's domain (e.g., `@TTYK.com`). Attackers register domains that closely resemble legitimate ones, such as `ITTYK.com`, to trick victims into believing the email is from a trusted system.

**2. The suspicious File Extension (.docm)**
* The attachment `Invoice_INV-992_Details.docm` ends in **.docm**. Legitimate invoices are almost always **.pdf** files. The extension **.docm** stands for "Word Document with Macros." This indicates the file contains executable scripts. If you open it, the document will likely ask you to "Enable Content," which will trigger the attack chain.

**3. The "Password Protection" Trick**
* The email locks the file with the password `SecurePay2025`. While this looks like a security measure for "sensitive banking details," it is actually an **evasion tactic**. Most email security scanners (antivirus and firewalls) cannot inspect the contents of an encrypted or password-protected file. By locking the document, the attacker ensures their malware bypasses the company's filters and lands directly in Clara's inbox.

**4. Urgency and Panic**
* The subject line uses **"URGENT"** and **"Overdue,"** combined with a high dollar amount (**$14,250.00**) and threats of a "payment hold." Phishing relies on emotional manipulation and impersonation to deceive victims. The attacker wants Clara to panic about the financial discrepancy and open the file immediately to "fix" the problem, rather than pausing to verify the sender's identity or confirm with the said sender.

**5. Targeted Personalization (Spearphishing)**
* The email addresses the victim by her full name (**Clara James**) and references her specific department. This indicates that the attacker had conducted thorough research on social media and the company's website. This specific detail builds trust, making the email appear to be a routine internal request rather than a random spam message.

## Part 2: Anatomy of a Click (What Happens Next)........

