# Incident: Wrong Branch Name Typo (`version/2.o` instead of `version/2.0`)

## Category
Branching / Naming Error

## Tags
["git", "branch", "naming", "typo", "recovery"]

## 1. Metadata
- **Component:** Git / Branching  
- **SEV Level:** SEV-3 (Low, Localized)  
- **Date:** 2025-11  
- **Environment:** Local development  
- **Owner:** Manasa  
- **Reporter:** Self  

---

## 2. Summary
A branch was mistakenly created as `version/2.o` instead of `version/2.0`, leading to confusion during later development and versioning tasks.

---
## 3. Pattern
- Human typo during manual branch creation  
- No branch name validation  
- Missed decimal placement (o vs 0)  

---
## 4. Symptoms
- Expected branch not found  
- CI pipeline expecting `version/2.0` didn’t detect branch  
- `git branch` showed unexpected variant  

---

## 5. If You See This Again, Try This First
1. Run `git branch -a`  
2. Search for similar-looking branch names  
3. Check reflog for recent branch pointer creation  
4. Validate semantic version naming manually  

---

## 6. Impact
- No production impact  
- Local development blocked temporarily  
- Versioning flow disrupted  
- Required cleanup to avoid confusing future branch history  

---

## 7. Pre-Checks
1. Confirm current branch using `git status`  
2. List branches using `git branch -a`  
3. Validate naming pattern with semantic versioning rules  
4. Check commit history with `git log --oneline`  
5. Ensure no uncommitted local changes  

---

## 8. Root Cause Analysis
### Git State
- Incorrect branch created due to a typo (`o` instead of `0`)  
- No branch protection rules for naming patterns  

### Infra/Pipeline State
- Not applicable  

### App State
- Not applicable  

---

## 7. Root Cause Analysis
- Wrong branch name created → new pointer created  
- Correct semantic version not followed  

**Root cause:** Typo + manual branch creation without automation.

---

## 8. Timeline
- 12:10 - Attempted to switch to version/2.0
- 12:11 - Git showed branch not found
- 12:12 - Ran `git branch -a` → found version/2.o
- 12:13 - Recognized typo
- 12:14 - Found correct commit reference using git reflog
- 12:16 - Created correct branch from commit
- 12:18 - Deleted incorrect typo branch


---

## 9. Fix Applied

### 1. Create the correct branch
```bash
git checkout -b version/2.0 <correct-commit-hash>
```

### 2. Delete wrong branch
```bash
git branch -D version/2.o
```

### 3. Verify
```bash
git branch -a
```

### 10. Validation
- Correct branch appeared in `git branch -a`.
- History matched expected versioning flow.
- No orphaned commits left behind.

### 11. Prevention
- Add personal naming checklist for version branches.
- Add local Git hook to validate branch names (optional).
- Prefer creating branches via scripts or Makefile tasks.
- Use tab-completion to avoid typos.

### 12. Lessons Learned
- Tiny typos can have cascading effects in release workflows
- `git reflog` is essential for recovering accidental branch creations
- Even “small” mistakes deserve documentation to avoid future repetition