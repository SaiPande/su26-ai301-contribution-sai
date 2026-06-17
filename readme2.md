Phase II: Reproduce & Plan
📖 What This Phase Is About
Phase II is where you prove — to yourself and to your mentors — that you actually understand the issue you selected. You do this by reproducing it locally on your own machine and writing a concrete plan for how you intend to fix it.

This is the most technically demanding setup phase of the program. You will set up a local development environment, trigger the bug or behavior described in your issue, and then write a plan that shows you understand the root cause and have a realistic path to solving it.

If Phase I was about choosing wisely, Phase II is about proving you can work with what you chose.

Target duration: 3–7 days.

Prerequisites: Before starting Phase II, you should have completed Phase I (issue selected, README updated) and have your GitHub account set up and project forked. If either is incomplete, go back and finish them first.

🏁 What "Done" Looks Like
To complete Phase II, you need to do three things:

1. Update your Contribution README with content under these template headers:

Clear numbered reproduction steps (someone else should be able to follow them without extra research) in Reproduction Process under Steps to Reproduce
Add a link to your branch in your fork in Reproduction Evidence
Write a short solution plan (bullet list or paragraph describing your intended approach) under Implementation Plan
2. Submit your check-in form and indicate "Phase II Complete."

3. (Recommended) Announce your Phase II completion in #dts-su26-ai301-celebration with a brief summary of your reproduction and plan.

No pull request. No finished code. Just proof that you can reproduce the problem and a plan that makes sense.

🗺️ Step-by-Step Procedure
Step 1: Set Up Your Local Development Environment (1–4 hours)
This is the step where most students hit friction. The setup experience will vary significantly depending on which project you chose — and this variability is intentional to teach you what real-world open source contribution looks like.

GitHub projects each define their own setup process. Here's what you'll encounter:

Best Case: The Project Has a Dev Container
Some well-maintained projects include a devcontainer.json file — VS Code's all-in-one dev environment. If your project has this:

Install VS Code and the Dev Containers extension

Clone your fork:

git clone https://github.com/<your-username>/<project-name>.git
cd <project-name>
VS Code will prompt you to "Reopen in Container" — do it

The container spins up with all dependencies pre-configured

Typical Case: README Setup Instructions
Most projects have a README.md or CONTRIBUTING.md with setup instructions. Quality varies widely — some are excellent, others are outdated.

Clone your fork and read the README thoroughly before running anything
Follow the setup steps for your OS
When you hit a dependency error (and you likely will), search for the exact error message online or paste it into Claude Code for help
Document every error and fix in your Contribution README — this is genuinely valuable for future contributors
Worst Case: Sparse or Outdated Documentation
If the setup docs are missing or outdated, you're largely on your own — just like a new hire on a team often is. Strategies:

Check the project's GitHub Issues for setup-related questions from other contributors
Check the Discussions tab if the project has one
Look at CI/CD config files (.github/workflows/) — they often reveal the correct build and test commands
Ask in the issue comments or project Discussions tab
This step is the most important qualifier from Phase I. When you selected your issue, one of the key criteria was whether the project has clear setup documentation. If you're spending more than 4 hours on setup with no progress, the project may not be suitable for this program cycle. Post in #dts-su26-ai301-solution-planning in Slack with your error details, and consider whether to pivot to a better-documented project. Pivoting now is likely better than trying to push through if you might get stuck later after more time investment, especially if this is your first issue.

Common Setup Issues and Fixes
Problem	Fix
Node/npm version mismatch	Check .nvmrc or .node-version and install that version via nvm
Python version mismatch	Check .python-version and use pyenv to install the correct version
Missing environment variables	Look for .env.example — copy it to .env and fill in any required values
Package install failures	Read the error carefully — usually a missing system dependency. The error message usually names it
Port already in use	Another process is using the dev server port. Find and kill it, or configure a different port
Tests won't run	Look for a Makefile or scripts in package.json — run make test or npm test respectively
If setup fails after 4 hours of effort: Stop. Ask for help. Post your error logs in #dts-su26-ai301-solution-planning in Slack. Include your OS, the project name, and the last 20 lines of error output. If class will meet within 24 hours, you can also ask for help during coworking time. Don't wait longer than that, though!

Step 2: Create Your Working Branch (10 minutes)
Note: If the project uses a branch other than main as its default (some use develop or trunk), check the repository's default branch on GitHub and use that for these commands instead of main.

Once your environment is running, create a branch in your fork:

If you haven't already cloned your fork, go back to step 1 at the top of this procedure.

Make sure you're up to date:

git checkout main
git pull origin main
Create a working branch named after your issue:

git checkout -b fix-issue-XXXXX
Push (VS Code's UI says Publish) the branch so it exists on the remote:

git push origin fix-issue-XXXXX
Step 3: Reproduce the Issue (1–2 hours)
Reproducing the issue means you can trigger the exact behavior described in the issue — consistently, not just once. This is critical because:

It proves the issue still exists (it may have been fixed upstream)
It gives you a baseline to test your fix against
Your reproduction steps become documentation for your README
How to Reproduce
Read the issue description again. Look for reproduction steps if they're provided. If they aren't, look in the comments — maintainers or reporters often clarify steps there. Check if any maintainers commented on the issue with hints or pointers.
Follow the steps exactly in your local environment. Navigate to the right page, enter the specified input, trigger the action.
Document what you observe. Write down:
What you expected to happen (the correct behavior)
What actually happened (the bug)
Any error messages, console output, or screenshots
Reproduce it at least twice to confirm it's consistent, not a fluke.
If you can't reproduce it: The issue may have been partially fixed, or your environment may differ from the reporter's. Check the issue comments for version information. Ask in #dts-su26-ai301-solution-planning or comment on the issue itself asking for clarification.
Using AI to Help Reproduce
Paste the issue URL or description into Claude Code or other AI tools and ask:

"Based on this issue, what files are likely involved?"
"What steps would reproduce this bug in your local environment?"
"Where in the codebase should I look for the relevant code?"
AI won't replace your own investigation, but it can point you to the right files and save hours of manual code-searching.

Step 4: Write Your Solution Plan (1–2 hours)
Now that you can reproduce the issue, think through how to fix it before writing any code. A plan prevents wasted effort and gives mentors something concrete to review.

Your plan should answer four questions:

1. What's the root cause? Why is this happening? Trace the behavior to the specific code (file, function, line if possible) that's responsible. Use AI tools to help navigate the codebase.

2. What's your proposed fix? Describe the change at a high level. "I will modify the validation method in user.rb to check email format before saving" is good. "I will fix the bug" is not.

3. What files will you touch? List the specific files you expect to modify. This helps mentors validate your approach.

4. How will you verify it works? What tests will you write or run? How will you confirm the bug is fixed and nothing else is broken?

The UMPIRE Framework (Adapted)
Use this structure to organize your plan in your README:

Understand: Restate the problem in your own words. What's broken? What should happen instead?
Match: What similar patterns or solutions exist in the codebase? Find a related piece of code that does something similar to what you need.
Plan: Step-by-step implementation plan. What will you modify, add, or remove?
Implement: (You'll do this in Phase III — leave a placeholder link for your branch)
Review: How will you self-review against the project's contribution guidelines (look for CONTRIBUTING.md in the project root)? Review the project's commit message and pull request conventions now so you're prepared.
Evaluate: What tests will confirm your fix works? Most projects require automated testing — check the project's contribution guidelines for what kind of tests are expected.
Step 5: Update Your README and Check-In (30 minutes)
Add or update the following sections of your Contribution README:

Under Reproduction Process:

Environment Setup: Notes on challenges you faced and how you solved them (any errors, how you resolved them)
Steps to Reproduce: Numbered steps another person could follow
Branch Link: Direct link to your working branch in your fork
Under Solution Approach:

Implementation Plan — Your UMPIRE-based plan or equivalent
Then submit your contribution readme and indicate "Phase II Complete."

⚠️ Slippery Spots
🟡 "I can't get the development environment running after 2+ days."
What's happening: Environment setup friction. This is the most common Phase II blocker. Fix: If you're completely stuck on setup and can't find help in the issue tracker or project Discussions, this may be a signal that the project isn't set up for easy external contribution. Post in #dts-su26-ai301-solution-planning with your OS, the project name, and the full error. It's not too late to change repositories if this one isn't ready for you to learn!

🟡 "I can't reproduce the issue — it seems to work fine for me."
What's happening: Environment mismatch, or the issue has been partially fixed. Fix: Check the issue comments for specific version numbers or browser requirements. Run git log on the relevant file to see if recent commits may have addressed it. Check if any maintainers have commented with version information or clarifications. If you've genuinely confirmed the issue no longer exists, comment on the issue to report your findings (this is itself a valuable contribution!), then go back to Phase I and select a new issue.

🟡 "I can reproduce it but I have no idea what's causing it."
What's happening: This is normal! Large codebases are hard to navigate, especially unfamiliar ones. Fix: Use AI tools to trace the code path. Load the relevant file(s) into Claude Code and ask: "What does this function do, and what could cause [symptom]?" Look at git blame on the relevant lines to see when they were last changed and why. Check if the issue has labels pointing to a specific area. Ask in the project's Discussions tab or issue comments for pointers.

🟡 "I jumped straight to coding without planning."
What's happening: The temptation to start fixing before understanding. This almost always leads to rework. Fix: Stop coding. Go back to Step 4. Write the plan first. It doesn't need to be long — even a 5-bullet plan is better than none. Your Phase III work will be faster and more focused with a plan in hand. Jumping straight to coding without a plan is especially risky when you code with AI assistance — it often leads to overly-verbose or non-standard implementations.

🟡 "My plan seems too simple — is that okay?"
What's happening: You may have chosen a well-scoped issue. That's good. Fix: Yes, simple plans are fine. "Fix typo in validation method, add unit test, verify existing tests pass" is a perfectly valid plan. Complexity is not a virtue in open source contributions.

✅ You're on the Right Track If...
✅ You can trigger the issue behavior on-demand in your local environment
✅ Someone else could follow your reproduction steps and see the same result
✅ You can name the specific file(s) and function(s) involved in the bug
✅ Your solution plan identifies a root cause, not just a symptom
✅ You documented your environment setup challenges (this helps others and shows mentors you're engaged)
📊 Strong vs. Weak README Examples
These examples don't use the contribution_readme.md template exactly, but should give you an idea of the level of specificity and detail we're looking for.

✅ Strong Example
## Reproduction Process

### Environment Setup
Used the project's dev container with VS Code — had it running in about 90 minutes. Only 
issue was needing to install the Dev Containers extension first.

Working branch: https://github.com/<your-username>/<project-name>/tree/fix-issue-12345

### Steps to Reproduce
1. Navigate to http://localhost:3000/profile/edit
2. In the email field, enter "notanemail"
3. Click "Save changes" button
4. **Expected:** Validation error appears, form doesn't submit
5. **Actual:** Form submits successfully, "notanemail" is saved to database

### Solution Plan
**Understand:** The profile edit form accepts invalid email formats without 
client-side or server-side validation feedback to the user.

**Match:** The `project.rb` model has a similar email validation pattern at 
line 89 that correctly rejects malformed emails.

**Plan:**
1. Fix the typo in `user.rb` line 45 (`validate_emai` → `validate_email`)
2. Add 3 unit tests in `spec/models/user_spec.rb`
3. Run full test suite to confirm no regressions

**Review:** Will self-review against project CONTRIBUTING.md and 
commit message conventions before opening PR.

**Evaluate:** Manual test reproducing steps 1-3 above should now show 
validation error. All existing tests should continue to pass.
Why this works: Specific setup path documented, branch in fork, numbered reproduction steps anyone could follow, root cause identified with file/line references, plan tied to UMPIRE framework with review preparation.

❌ Weak Example
I set up the development environment and was able to reproduce the bug. 
The email validation doesn't work.

## Solution Approach
I'll fix the email validation code and add some tests.
Why this fails: No detail on setup path, no branch link, no specific reproduction steps, no root cause analysis, vague plan. A person reading this would need to ask many follow-up questions.

📣 When and How to Escalate
Phase II should take 3–7 days. The biggest blocker is typically environment setup.

Level 1: Peer + AI Help (anytime)
Post in #dts-su26-ai301-solution-planning in Slack for phase-specific help. Include your error messages, OS, and what you've tried.

Level 2: Coworking Time in Class (Wed)
Setup issues are the top use case for hands-on assistance. Share your screen and debug live with your classmates.

Skip this step if it would add a delay, though.

Level 3: Direct Staff Support (Day 3+ with no environment, Day 7+ with no plan)
If you don't have a working environment by Day 3 of Phase II, tag @ai-help in your Slack thread for assistance from CodePath Staff with debugging or choosing a new issue. If you don't have a plan by Day 7, tag @ai-help in your Slack thread for a template and examples.

📚 Resources

Phase II Deliverables Summary
Deliverable	Where
Reproduction steps + branch link + solution plan	Your Contribution README on GitHub
Phase II marked complete	Click Submit in the top right of this page
Milestone announced	#dts-su26-ai301-celebration in Slack (recommended)
What Happens Next
Once your README has reproduction steps, a branch link, and a solution plan — and your check-in is submitted — you're done with Phase II. Move directly to Phase III: Build, where you'll implement your solution, write tests, and push working code.

Don't wait for permission to start coding. If your plan is solid, begin Phase III immediately.
