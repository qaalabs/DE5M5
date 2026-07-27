## Local Development Setup

- Open Terminal
- Git config first (email, name) - same two lines as M4
- Clone their repo: `git clone https://github.com/USERNAME/qa-library-pipeline.git` - public repo, no auth needed to clone
- `git remote -v` after cloning - confirms origin is set before they start editing
- `git push --dry-run` - safe, pushes nothing, but exercises the actual push-auth path. This is where the GitHub sign-in popup (Git Credential Manager) fires on a fresh VM - walk them through it here so it doesn't derail the real push later this afternoon
- Navigate in, explore the structure locally
- Note: learners with resetting VMs need to clone again each morning
