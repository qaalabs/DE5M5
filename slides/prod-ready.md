---
title: Production-Ready Code
---

# What makes code "production-ready"?

Before we look at the agenda for this week - a quick question.

> You've built something that works on your laptop.

<mark>What would need to be true before we could trust it in a real environment?</mark>

🔻
----
# What makes code "production-ready"?

### What would need to be true before we could trust it in a real environment?

Think about:

- who else needs to run it?
- what happens when something goes wrong?
- how we know it still works after a change?
- how we move it safely from development to production?

---

# Production-Ready Code

| Criterion | What it means |
|-----------|---------------|
| **Tested** | >70% coverage - your code is covered by tests |
| **Version controlled** | Full history, rollback, team collaboration |
| **Documented** | Someone else can run it without you |
| **Automated deployment** | No manual steps between merge and production |
| **Error handling** | Fails gracefully with a clear reason, not silently |
| **Security considered** | Access controls, no credentials in code |

---

# The bar we're working to

"It runs without errors" is not enough.

**For this module, production-ready means:**

> another developer could understand it, test it, run it, deploy it, and troubleshoot it without relying on you being there.

---

# Production-Ready Code

## Which of these is most often missing in code you work with?

- testing?
- documentation? 
- error handling? 
- deployment? 
- security? 
- or maintainability?

