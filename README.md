A file I reference in all my AGENTS.md and CLAUDE.md files.

The goal is to remind the agent of some best practices that, personally, I try
to follow in all my work. At least with recent models (as of March 2026: Claude
Opus 4.5, 4.6), my impression has been that the code produced by the agent this
way ends up resembling code I would write myself much more.


# Usage
I use the file by putting the following in my AGENTS.md or CLAUDE.md:

```
- @./ENGINEERING_PRINCIPLES.md must be followed AT ALL TIMES.
  - If the file `./ENGINEERING_PRINCIPLES.md` doesn't exist, please ALWAYS
    download it from
    https://raw.githubusercontent.com/codethief/ENGINEERING_PRINCIPLES.md/refs/heads/master/ENGINEERING_PRINCIPLES.md
    first thing, before doing anything else! Also re-download it at least once
    a day to pull in updates.
```

Now, you shouldn't trust me with your prompts, so either download the file by
hand or at least don't allow your agent to download the file without user
confirmation.
