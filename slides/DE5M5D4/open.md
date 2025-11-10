### *title*

The aim of day 4 is ...

https://qaalabs.github.io/DE5M5/trainer

---

## **Module 5: Day 4 - Polish & Present**

**Theme**: Final refinements, security, and showcasing the complete data product

---

### **9:30-9:50 | Welcome & Final Day Overview (20 mins)**
**Format**: Trainer-led (WebEx screen share)

**Content**:
- Welcome to the final day!
- Week recap - The journey:
  - **Day 1**: Planning, architecture, setup
  - **Day 2**: Building tested Python code
  - **Day 3**: Automation and cloud deployment
  - **Day 4**: Polish and present
- Today's goals:
  - Morning: Final refinements, security, documentation
  - Afternoon: Showcase your work!
- **Key message**: "You've built something real. Today we celebrate it!"

**Quick Check-In** (10 mins):
Each learner shares:
- Project status: Ready to present?
- What are you most proud of?
- Any last-minute issues?
- Presentation nerves? (It's normal!)

**Presentation Logistics**:
- 15 minutes per person (10 min present + 5 min Q&A)
- Order: Will decide after morning session
- Recording? (Ask if they want presentations recorded)
- Peer feedback forms

**Artifacts Needed**:
- 📊 Slide deck: Day 4 overview
- 📊 Slide: Week journey visualization
- 📄 Presentation evaluation rubric (share with learners)
- 📄 Peer feedback form

---

### **Session 1: 9:50-10:40 | Security & Access Control (50 mins)**
**Format**: Trainer demo (20 mins) → Learner implementation (30 mins)

**Learning Objectives**:
- Understand data security principles
- Implement basic security controls in Fabric
- Document security practices
- Complete S13 KSB requirement

**Trainer Demo** (20 mins):

**Part 1: Why Security Matters** (5 mins)

**The problem**:
```
Library Data Includes:
- Member IDs (PII)
- Checkout history (reading habits - sensitive!)
- Branch-specific data (should branches see each other's data?)

Without Security:
- Anyone with access sees everything
- Data breaches possible
- Compliance issues (GDPR, Data Protection Act)
```

**Security layers**:
1. **Workspace access** - Who can enter?
2. **Item permissions** - Who can view/edit notebooks?
3. **Row-level security** - Who can see which rows?
4. **Column-level security** - Hide sensitive columns

---

**Part 2: Implementing Security in Fabric** (15 mins)

**Live demo in Fabric**:

**1. Workspace Roles** (5 mins)
- Show workspace settings
- Explain roles:
  - **Admin**: Full control
  - **Member**: Can create and edit
  - **Contributor**: Can edit existing
  - **Viewer**: Read-only

**2. Row-Level Security (RLS)** (10 mins)

**Create a security table**:
```sql
-- In Fabric notebook
CREATE TABLE IF NOT EXISTS user_branch_access (
    user_email STRING,
    branch_id STRING
);

-- Insert sample access rules
INSERT INTO user_branch_access VALUES
    ('manager@library.com', 'ALL'),
    ('stratford@library.com', 'BR001'),
    ('eastham@library.com', 'BR002');
```

**Create a secure view**:
```sql
-- View with RLS
CREATE OR REPLACE VIEW silver_circulation_secure AS
SELECT 
    c.transaction_id,
    c.member_id,
    c.isbn,
    c.checkout_date,
    c.return_date,
    c.branch_id
FROM silver_circulation c
WHERE 
    -- Allow if user has 'ALL' access
    EXISTS (
        SELECT 1 FROM user_branch_access 
        WHERE user_email = current_user() 
        AND branch_id = 'ALL'
    )
    OR
    -- Or if user has specific branch access
    EXISTS (
        SELECT 1 FROM user_branch_access 
        WHERE user_email = current_user() 
        AND branch_id = c.branch_id
    );
```

**Test it**:
```sql
-- Manager sees all branches
SELECT DISTINCT branch_id FROM silver_circulation_secure;

-- Branch user sees only their branch
-- (Would need to test with different user)
```

**3. Document Security Practices** (5 mins)

**Create `docs/SECURITY.md`**:
```markdown
# Security Implementation

## Access Control

### Workspace Level
- Workspace admins: [List admins]
- Members: [List members]
- Viewers: [List viewers]

### Data Level
- Row-level security implemented on circulation data
- Branch managers can only see their branch data
- System administrators can see all data

## Sensitive Data Handling

### Personal Identifiable Information (PII)
- Member IDs are pseudonymized
- No names or contact details stored in pipeline
- Access logged and auditable

### Data Classification
- **Confidential**: Member checkout history
- **Internal**: Branch statistics
- **Public**: Aggregated counts (no PII)

## Compliance
- GDPR compliant: Right to erasure implemented
- Data Protection Act 2018 compliant
- Retention policy: 7 years for audit trail

## Audit Trail
- All data access logged in Fabric
- Regular security reviews conducted
- Incident response plan documented

## Future Enhancements
- [ ] Implement column-level encryption
- [ ] Add data masking for non-production environments
- [ ] Implement multi-factor authentication
- [ ] Regular penetration testing
```

---

**Learner Activity** (30 mins):

**Task: Implement and Document Security**

**Instructions**:
```markdown
## Part 1: Understand Your Data (5 mins)

Review your data:
- What's sensitive? (PII, confidential info)
- Who should access what?
- What are the risks?

## Part 2: Implement Basic Security (15 mins)

Choose ONE to implement:

Option A: Row-Level Security (Recommended)
- Create user_branch_access table
- Create secure view with RLS
- Test with queries

Option B: Documentation Only
- If Fabric access limited, document how you WOULD implement RLS
- Include SQL code in documentation
- Explain the security model

## Part 3: Document Security Practices (10 mins)

Create `docs/SECURITY.md`:
- What sensitive data exists?
- How is it protected?
- Who has access to what?
- Compliance considerations
- Future security improvements

Use the template provided!
```

**Trainer Support**:
- Help with SQL syntax for RLS
- Review security documentation
- Discuss real-world security scenarios
- Validate security approaches

**Q&A Checkpoint**:
> - "What sensitive data did you identify?"
> - "How are you protecting it?"
> - "Show me your SECURITY.md"
> - "What would you do differently in production?"

**Artifacts Needed**:
- 📊 Slides: Data security principles
- 📊 Slides: Row-level security explanation
- 📊 Slides: Fabric security features
- 💾 Sample RLS SQL code
- 📄 SECURITY.md template
- 📄 Security implementation checklist

---

### **10:40-11:00 | Break (20 mins)**

---

### **Session 2: 11:00-12:15 | Final Polish & Technical Debt (75 mins)**
**Format**: Learner-led work with trainer support

**Learning Objectives**:
- Complete all documentation
- Identify and document technical debt
- Ensure everything works end-to-end
- Add final touches to make project presentation-ready

**Trainer Introduction** (10 mins):

**What is Technical Debt?**
```
Technical Debt = Shortcuts you took that will need fixing later

Examples:
- "I hard-coded this path" → Should be configurable
- "I skipped error handling here" → Should add try/except
- "This function is too long" → Should be refactored
- "Coverage is 72% but that function isn't tested" → Should add tests

NOT FAILURES! Just honest reflection.
```

**Why document it?**
- ✅ Shows maturity (you recognize imperfections)
- ✅ Helps next developer (or future you!)
- ✅ Shows what you'd do with more time
- ✅ Part of continuous improvement (K28)

**Today's task**: Final polish + honest reflection

---

**Learner Activity** (65 mins):

**Task: Complete Your Project**

**Checklist**:
```markdown
## 1. Documentation Complete (20 mins)

### README.md
- [ ] Project overview and purpose
- [ ] Architecture diagram visible
- [ ] Setup instructions (local and Fabric)
- [ ] How to run tests
- [ ] Coverage badge/stats
- [ ] CI/CD status badge
- [ ] How to deploy to Fabric
- [ ] Sample queries to run

### docs/architecture/
- [ ] Architecture diagram (PNG/PDF)
- [ ] ADRs (Architecture Decision Records)
- [ ] Data flow documentation

### docs/SECURITY.md
- [ ] Security implementation documented
- [ ] Access control explained
- [ ] Compliance notes

### Code Documentation
- [ ] All functions have docstrings
- [ ] Comments explain WHY, not WHAT
- [ ] Complex logic is explained

---

## 2. Technical Debt Documentation (15 mins)

Create `docs/TECHNICAL_DEBT.md`:

```markdown
# Technical Debt

## Items to Address

### High Priority
1. **Hard-coded file paths**
   - Location: `ingestion.py` line 45
   - Issue: Paths won't work on other systems
   - Solution: Use config file or environment variables
   - Effort: 2 hours

### Medium Priority
2. **Incomplete test coverage for Excel loading**
   - Location: `test_ingestion.py`
   - Issue: Only 65% coverage on `load_excel()`
   - Solution: Add tests for edge cases
   - Effort: 1 hour

### Low Priority
3. **Long function in cleaning.py**
   - Location: `standardize_dates()` is 45 lines
   - Issue: Difficult to test individual parts
   - Solution: Break into smaller functions
   - Effort: 1 hour

## Limitations
- Only handles 3 data file types (CSV, JSON, Excel)
- No streaming support (batch only)
- No retry logic for failed transformations
- Security is basic (no encryption at rest)

## Future Enhancements
- Add data quality dashboard
- Implement real-time processing
- Add more sophisticated validation rules
- Create data lineage visualization
```

---

## 3. End-to-End Testing (15 mins)

Run everything to confirm it works:

### Locally
```bash
# Tests pass?
pytest tests/ -v

# Coverage acceptable?
pytest tests/ --cov=src --cov-report=term

# CI/CD passing?
# Check GitHub Actions tab
```

### In Fabric
- [ ] Start Fabric playground
- [ ] Open notebook
- [ ] Run all cells
- [ ] Verify Delta table created
- [ ] Run sample queries
- [ ] Screenshot results (for presentation!)

---

## 4. Presentation Preparation (15 mins)

### Create Presentation Materials
- [ ] Slide deck OR
- [ ] Demo script (what to show and in what order)

### Test Your Demo
- [ ] Practice running your pipeline
- [ ] Take screenshots of key results
- [ ] Prepare backup (what if Fabric fails?)
- [ ] Time yourself (aim for 10 minutes)

### Prepare for Questions
Think about:
- What was your biggest challenge?
- What would you do differently?
- What are you most proud of?
- How does this apply to your work?
- What did you learn?

**Trainer Circulates**:
- Do final checks with each learner
- Review documentation quality
- Help identify technical debt
- Provide encouragement
- Take note of interesting demos to highlight

**Q&A Checkpoint**:
> - "Is your README complete?"
> - "Show me your technical debt list"
> - "Does your end-to-end pipeline work?"
> - "Are you ready to present?"
> - "What are you nervous about?"

**Artifacts Needed**:
- 📄 TECHNICAL_DEBT.md template
- 📄 Documentation completeness checklist
- 📄 README.md exemplar (good example)
- 📄 Presentation preparation guide

---

### **12:15-13:15 | Lunch (60 mins)**

**During lunch**: 
- Determine presentation order (random? volunteer?)
- Set up evaluation forms
- Test screen sharing setup

---

### **Session 3: 13:15-14:30 | Presentations - Part 1 (75 mins)**
**Format**: Learner presentations

**Structure**: 15 minutes per learner
- 10 minutes: Presentation
- 5 minutes: Q&A and feedback

**With 7 learners**:
- Session 3: Learners 1-4 (60 mins + 15 buffer)
- Session 4: Learners 5-7 (45 mins) + Wrap-up (15 mins)

---

**Presentation Format**:

Each learner presents:

**1. Introduction** (1 min)
- Name and project title
- Brief overview of what they built

**2. Business Problem** (1 min)
- Library's data quality challenge
- Why this matters

**3. Solution Architecture** (2 mins)
- Show architecture diagram
- Explain: Bronze → Silver → Gold
- Highlight key design decisions

**4. Live Demo** (4 mins)
- **Option A**: Show working pipeline in Fabric
  - Install package
  - Run transformations
  - Query results
- **Option B**: Show local development + explain deployment
  - Run tests locally
  - Show GitHub Actions passing
  - Explain Fabric deployment

**5. Technical Approach** (2 mins)
- Code quality: Show test coverage
- CI/CD: Show GitHub Actions workflow
- Security: Explain security measures
- Show one interesting piece of code

**6. Reflections** (1 min)
- Biggest challenge overcome
- What you'd do differently
- Key learning
- Technical debt identified

**7. Q&A** (5 mins)
- Trainer asks questions
- Peer questions (optional)
- Feedback

---

**Evaluation Criteria** (Share with learners beforehand):

```markdown
# Presentation Rubric

## Technical Implementation (40%)
- [ ] Code quality and organization (10%)
- [ ] Testing coverage and quality (10%)
- [ ] CI/CD working (10%)
- [ ] Security implementation (10%)

## Documentation (20%)
- [ ] README clarity (10%)
- [ ] Architecture documentation (5%)
- [ ] Technical debt identified (5%)

## Demonstration (20%)
- [ ] Working pipeline (10%)
- [ ] Clear explanation of functionality (10%)

## Communication (20%)
- [ ] Clear presentation structure (5%)
- [ ] Technical depth appropriate (5%)
- [ ] Handles questions well (5%)
- [ ] Reflection and learning demonstrated (5%)

## Bonus Points
- [ ] Particularly creative solution
- [ ] Exceptional code quality
- [ ] Outstanding documentation
- [ ] Great debugging story
```

---

**Trainer Role During Presentations**:
- ✅ Timekeeping (give 2-min warning)
- ✅ Ask probing questions
- ✅ Highlight good practices
- ✅ Take notes for feedback
- ✅ Encourage peer questions
- ✅ Create supportive atmosphere

**Encourage Peers**:
> "After each presentation, let's give a quick round of applause. Also, feel free to ask questions - we're all learning from each other!"

---

**Presentation Order** (Decide during lunch):

**Option 1: Random**
- Draw names from hat
- Fair and unbiased

**Option 2: Volunteer**
- Who wants to go first?
- Usually someone confident volunteers

**Option 3: Reverse confidence**
- Most nervous go first (get it over with!)
- Most confident go last (set a high bar)

**My recommendation**: Ask for 2 volunteers (first and last), random for middle 5

---

**After Each Presentation**:

**Immediate Feedback** (2 mins):
- Trainer highlights 2-3 strengths
- One area for improvement (constructive)
- Peer comments (1-2 quick observations)

**Peer Feedback Form**:
```markdown
# Peer Feedback - [Presenter Name]

## What impressed you most?


## One thing you learned from this presentation:


## One question you still have:


## One suggestion for improvement:

```

---

**Artifacts Needed**:
- 📊 Presentation guidelines slide
- 📄 Presentation rubric (printed/shared)
- 📄 Peer feedback forms
- 📄 Presentation order list
- ⏱️ Timer (visible to presenter)

---

### **Session 4: 14:50-15:50 | Presentations - Part 2 & Wrap-Up (60 mins)**
**Format**: Learner presentations (45 mins) + Module wrap-up (15 mins)

**Presentations 5-7** (45 mins):
- Same format as Session 3
- 15 minutes each
- Continue evaluation

---

**After Final Presentation** (5 mins):
- Applause for everyone
- Quick break while you prepare wrap-up

---

### **Module 5 Wrap-Up & Celebration (10 mins)**

**Content**:

**1. Congratulations!** (2 mins)
> "You've completed Module 5: Building Data Products!
> 
> Four days ago, you had an idea. Today, you have:
> - ✅ A working Python package
> - ✅ Comprehensive tests (70%+ coverage)
> - ✅ Automated CI/CD
> - ✅ Cloud deployment to Fabric
> - ✅ Security implementation
> - ✅ Complete documentation
> 
> This is REAL data engineering work!"

---

**2. What We Learned** (3 mins)

**Day 1**: Planning and architecture
- Design before building
- Documentation matters
- Version control from day 1

**Day 2**: Writing production-quality code
- Testing isn't optional
- pandas.testing.assert_frame_equal() ✅
- Pure functions (.copy()!)

**Day 3**: Automation and deployment
- CI/CD catches bugs early
- Pull requests protect main branch
- Local development → Cloud production

**Day 4**: Polish and communication
- Security is part of the product
- Technical debt is normal (and worth documenting)
- Presenting your work is a skill

---

**3. How This Connects** (2 mins)

**Module 3**: You learned ETL patterns
**Module 4**: You learned to plan
**Module 5**: You learned to BUILD ✅
**Module 6** (next): You'll learn to OPERATE

**The cycle**:
```
Plan → Build → Deploy → Monitor → Improve → Repeat
```

You've now completed: Plan → Build → Deploy!

---

**4. Real-World Relevance** (2 mins)

**What you built is how professionals work**:
- ✅ Test-driven development
- ✅ CI/CD pipelines
- ✅ Pull request workflows
- ✅ Cloud deployment
- ✅ Security-conscious design
- ✅ Documentation-first approach

**You can talk about this in interviews**:
> "I built a production-ready data pipeline with automated testing, CI/CD, and cloud deployment..."

---

**5. Next Steps** (1 min)

**Before Module 6**:
- Complete any final documentation
- Reflect on what you learned
- Review Module 6 pre-work (if any)

**Module 6 Preview**: Data Operations
- Monitoring pipelines
- Incident response
- Performance optimization
- DataOps principles
- **You'll operate what you've built!**

---

**6. Final Words** (1 min)

> "I'm proud of what you've accomplished. Building production data products in 4 days is no small feat. You should be proud too.
> 
> Keep this code - add it to your portfolio, your GitHub profile, show it to potential employers. This is proof of your skills.
> 
> Any final questions? ...
> 
> Great work, everyone. See you in Module 6!"

**Artifacts Needed**:
- 📊 Module 5 wrap-up slides
- 📊 Module 6 preview slide
- 📄 Module 5 reflection form (optional homework)
- 🎉 Celebration GIF or image (fun touch!)

---

### **15:50-16:00 | Informal Q&A and Goodbye (10 mins)**
**Format**: Open discussion

**Optional activities**:
- Share contact information
- Photo of the cohort (if appropriate)
- Collect feedback on the module
- Answer any lingering questions
- Personal congratulations

---

## Day 4 Artifacts Summary

### **Slides/Presentations** 📊
- [ ] Day 4 overview and week journey
- [ ] Data security principles
- [ ] Row-level security explanation
- [ ] Fabric security features
- [ ] Technical debt explanation
- [ ] Presentation guidelines
- [ ] Presentation rubric
- [ ] Module 5 wrap-up and accomplishments
- [ ] Module 6 preview

### **Code/Templates** 💾
- [ ] Sample RLS SQL code
- [ ] SECURITY.md template
- [ ] TECHNICAL_DEBT.md template
- [ ] README.md exemplar

### **Documents/Guides** 📄
- [ ] Presentation evaluation rubric
- [ ] Peer feedback forms
- [ ] Security implementation checklist
- [ ] Documentation completeness checklist
- [ ] Presentation preparation guide
- [ ] Module 5 reflection form

---

## Special Considerations for 7 Learners

### **Presentation Timing**:
**7 learners × 15 mins = 105 minutes**

**Session 3** (75 mins): 4 presentations
**Session 4** (45 mins): 3 presentations + wrap-up

**This fits perfectly!** ✅

### **Intimate Setting Advantages**:
- Can give detailed feedback to each person
- More time for Q&A if needed
- Can adjust timing if someone needs extra time
- Can have richer discussions
- Everyone can ask questions

### **Create Community**:
With only 7 learners:
- Encourage them to stay connected
- Suggest they share repos with each other
- Maybe create a cohort Slack/Teams channel
- They'll be great peer support for Module 6

---

## Post-Module 5 Activities (Optional)

### **For Learners** (Optional homework):
1. **Portfolio**: Add project to GitHub profile README
2. **LinkedIn**: Post about completion with screenshots
3. **Reflection**: Write a blog post about what they learned
4. **Improvement**: Address one item from technical debt

### **For You** (Trainer):
1. **Collect feedback**: What worked? What didn't?
2. **Review projects**: Identify common patterns for next cohort
3. **Update materials**: Note what to change for next time
4. **Prepare Module 6**: Based on what they've built

---

## Module 5 Complete! 🎉

### **What We've Planned**:
- ✅ **Day 1**: Foundation, setup, architecture (6 hours)
- ✅ **Day 2**: Building tested Python code (6 hours)
- ✅ **Day 3**: CI/CD and Fabric deployment (6 hours)
- ✅ **Day 4**: Security, polish, presentations (6 hours)

**Total**: 4 days, 24 hours of structured learning

### **What Learners Will Have**:
- ✅ Production-ready Python package
- ✅ 70%+ test coverage
- ✅ GitHub repository with CI/CD
- ✅ Deployed to Microsoft Fabric
- ✅ Security implemented
- ✅ Complete documentation
- ✅ Portfolio-worthy project

---
