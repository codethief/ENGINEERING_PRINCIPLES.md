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
    first thing, before doing anything else!
```

This way, I can manage the file centrally and in my downstream projects I can
just tell my agent to pull the file again whenever it was updated upstream.

Now, you shouldn't trust me with your prompts, so either put the file in your
own repo or at least don't allow your agent to download the file from my repo
without user confirmation.
