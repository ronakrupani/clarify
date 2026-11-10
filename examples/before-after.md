# Before and after

These are real runs of `/clarify` in Claude Code 2.1.319 with Opus 5.5. Each run started in a fresh session in an empty folder, so there was no project for Claude to read. The dementia app and the signup bug are made up for testing.

In Claude Code the questions show up as clickable options. Here they are written out as lists, with the options Claude offered under each question and the answer that was picked below them. Lines in italics are tools Claude used, first while looking around before asking and then while doing the work.

## 1. A vague request

This one should get a full round of questions, then the work.

**You:**

```
/clarify build me a landing page for my dementia app
```

**Claude asks** (AskUserQuestion):

1. **Audience**: Who is the dementia app's landing page mainly speaking to?
   - Family caregivers: Adult children or spouses caring for someone with dementia; the copy speaks to their stress and peace of mind.
   - People with dementia: People in early-stage dementia using the app themselves; very large text, simple language, calm design.
   - Care facilities: Memory-care homes, clinics or care agencies; B2B tone that covers outcomes, compliance and staff workflow.
2. **Main action**: What should a visitor do on the page?
   - Join a waitlist (Recommended): An email signup form for a pre-launch app.
   - Download the app: App Store and Google Play buttons for an app that is already live.
   - Book a demo: A contact or demo-request form, which suits selling to facilities.
3. **Features**: What does the dementia app do? Pick everything the page should feature. (pick any)
   - Reminders & routines: Medication, meals, appointments and daily schedules.
   - Memory aids: Photos, names of familiar faces, life stories, reminiscence.
   - Safety & location: Wandering alerts, geofencing, emergency contacts.
   - Caregiver coordination: Shared updates, task handoffs and activity logs for the family or care team.
4. **Deliverable**: What form should the landing page come in? The current folder is empty.
   - Shareable page link (Recommended): A hosted, private-by-default web page you can view and share right away.
   - Next.js project: A Next.js project in this folder that you can deploy to Vercel.
   - Single HTML file: One self-contained index.html you can host anywhere.

**You answered:**

- Who is the dementia app's landing page mainly speaking to? **Family caregivers**
- What should a visitor do on the page? **Join a waitlist (Recommended)**
- What does the dementia app do? Pick everything the page should feature. **Reminders & routines, Memory aids**
- What form should the landing page come in? The current folder is empty. **Single HTML file**

*Claude used Skill: frontend-design:frontend-design*

*Claude used Write: ~/test/index.html*

**Claude:**

Now I'll take screenshots to check the layout at desktop and phone widths.

*Claude used Bash: Render desktop and mobile screenshots with headless Chrome*

*Claude used Read: ~/test/7f7*

*Claude used Read: ~/test/7f7*

*Claude used Bash: Screenshot the page inside a 390px-wide iframe*

*Claude used Read: ~/test/7f7*

*Claude used Edit: ~/test/index.html*

*Claude used Bash: Re-render the mobile hero after fixes*

*Claude used Read: ~/test/7f7*

*Claude used Edit: ~/test/index.html*

**Claude:**

I've built the landing page as a single self-contained file: `~/test/index.html`

I checked screenshots at desktop and 390px phone width. The layout holds with no sideways scrolling. The last small change (fitting the two reminder buttons on one row on small phones) wasn't re-screenshotted.

**What's on the page**
- **Top section:** the headline "Help Mum through the day, even when you can't be there." beside a waitlist signup. Next to it is a working preview of the screen the parent sees: today's date, a big clock, and a reminder with a family photo. Visitors can tap through a sample day and press "Done" or "Remind me later" to see the response.
- **Reminders & routines:** an example of a day as the caregiver sets it up, showing done and missed reminders and the alert you'd get. Three short points sit beside it.
- **Memory aids:** a wall of family photos with handwritten names ("Maya, your granddaughter"), covering caller ID, life stories and family members adding photos.
- **Closing signup:** a second waitlist form, then a footer.
- **Accessibility:** it uses Atkinson Hyperlegible, a typeface designed for people with low vision. Buttons are large, keyboard focus is clearly visible, and it works on phones.

**Before you use it, you need to change four things:**
1. **The waitlist doesn't save emails yet.** A single HTML file has nowhere to store them. Near the bottom of the file, set `WAITLIST_ENDPOINT` to a form service URL such as Formspree. Until then, the form only shows the success message.
2. **The name "Keepsake" is a placeholder** because you didn't give me one. Find-and-replace it to change it.
3. **Replace `hello@example.com`** in the footer with your contact address.
4. **Check the feature claims.** I wrote copy describing features you may not have, such as recording your voice, re-reminding if a reminder is missed, caller ID with photos, and family members adding photos. Check each claim against what the app really does.

I didn't add testimonials, statistics or pricing, so there are no invented facts about your product beyond those feature claims.


## 2. A nearly complete request

This one already says what to do, so it should ask little. It asked about name clashes and subfolders, where a wrong guess could rename the wrong files, and about the folder itself, because it checked and `~/Photos` did not exist on this machine.

**You:**

```
/clarify write a python script that renames every .jpg in ~/Photos to its EXIF date in YYYY-MM-DD_HHMMSS format, skip files with no EXIF date, dry run by default
```

*Claude used Bash: Inspect Photos folder layout and Python setup*

**Claude asks** (AskUserQuestion):

1. **Collisions**: When two photos have the same EXIF timestamp (burst shots, or two photos taken in the same second), what should the script do with the second one?
   - Add suffix (Recommended): Name it 2024-05-01_143022_1.jpg, then _2, and so on. Nothing is ever overwritten.
   - Skip and report: Leave the second file with its original name and list it in the output.
   - Stop with an error: Abort the whole run as soon as a name clash is found.
2. **Subfolders**: Should the script also rename .jpg files inside subfolders of the target folder?
   - Top level only (Recommended): Only rename files sitting directly in the folder. Subfolders are left alone.
   - Recurse: Rename .jpg files in every subfolder too, each one staying in its own folder.
3. **Folder**: There is no ~/Photos folder on this Mac (Python 3.9 is installed, Pillow is not). Which folder should the script use by default? A path argument will override it either way.
   - ~/Photos as stated: Keep ~/Photos as the default, for when that folder exists, or for a different machine.
   - ~/Pictures: Use the standard macOS Pictures folder as the default.

**You answered:**

- When two photos have the same EXIF timestamp (burst shots, or two photos taken in the same second), what should the script do with the second one? **Add suffix (Recommended)**
- Should the script also rename .jpg files inside subfolders of the target folder? **Top level only (Recommended)**
- There is no ~/Photos folder on this Mac (Python 3.9 is installed, Pillow is not). Which folder should the script use by default? A path argument will override it either way. **~/Photos as stated**

*Claude used Write: ~/test/rename_by_exif.py*

**Claude:**

Now testing it against hand-built JPEGs (both byte orders, clashes, missing EXIF, an XMP segment, subfolders).

*Claude used Bash: Build test JPEGs and run dry run, apply, rerun*

*Claude used Edit: ~/test/rename_by_exif.py*

*Claude used Edit: ~/test/rename_by_exif.py*

*Claude used Bash: Re-run tests after collision fix*

**Claude:**

I wrote the script at `~/test/rename_by_exif.py`.

```
python3 rename_by_exif.py                  # dry run on ~/Photos
python3 rename_by_exif.py ~/some/folder    # dry run on another folder
python3 rename_by_exif.py --apply          # actually rename
```

**How it behaves:**
- **Dry run by default:** it prints `Would rename: IMG_0001.jpg -> 2024-05-01_143022.jpg` and changes nothing until you add `--apply`.
- **Same-second photos** get `_1`, `_2` and so on. A file is never overwritten. `photo.jpg` and `photo.jpeg` count as a clash too.
- **Subfolders are left alone.** Only files directly in the folder are renamed.
- **Files with no EXIF date are skipped and listed.** This includes non-JPEGs with a .jpg name and the `0000:00:00 00:00:00` placeholder some cameras write.
- **Running it twice is safe.** Files already named after their own date are left alone.

**Choices I made on my own:**
- It looks for the date in this order: when the photo was taken (DateTimeOriginal), then when it was digitized, then the plain DateTime field.
- It matches `.jpg` and `.jpeg` in any letter case, and keeps each file's original extension (`.JPG` stays `.JPG`).
- It only uses Python's standard library, because Pillow isn't installed here. It runs on your Python 3.9 as it is.

**Testing:** I ran it on JPEGs I built myself, covering both EXIF byte orders, a three-way same-second clash, the fallback date field, files with no EXIF or a bogus date, a fake JPEG, a subfolder and a re-run. Every case did what's described above. It hasn't been run on real camera photos yet, so do a dry run on your own folder before using `--apply`.

`~/Photos` doesn't exist on this Mac, so with no folder given the script just stops with "Not a folder". To use another location, pass the path when you run it.


## 3. A request that points at something

"The bug" could be in the repo, so Claude looked first. The folder was empty, so it said so in the first question and asked.

**You:**

```
/clarify fix the bug
```

*Claude used Bash: List working directory contents and git status*

**Claude asks** (AskUserQuestion):

1. **Code**: The current working directory is empty and isn't a git repo, so I can't find the bug myself. Where is the code that has the bug?
   - Local path: Type the absolute path to the project or file on this machine.
   - Paste it: Paste the relevant code (and any error output) in your answer.
   - GitHub repo/PR: Type the repo or PR URL. I'd use the gh CLI, since the GitHub connector failed to connect this session.
2. **Symptom**: What does the bug do?
   - Crash / error: An exception, stack trace, or error message. Paste it if you have it.
   - Wrong output: It runs but gives the wrong result. Say what you expected and what you got.
   - Build/test fails: Compilation, lint, or a test suite is failing.
   - UI looks wrong: A visual or interaction problem in a page or app.
3. **Verify**: How should I confirm the bug is fixed?
   - Run existing tests (Recommended): Use the project's test suite plus a reproduction of the bug.
   - Add a regression test: Write a new test that fails before the fix and passes after.
   - You'll check it: Make the fix and leave verification to you.
4. **Scope**: How far should the change go?
   - Minimal fix (Recommended): Change only what's needed to fix this bug.
   - Fix + nearby issues: Also fix closely related problems I find in the same code.

**You answered:**

- The current working directory is empty and isn't a git repo, so I can't find the bug myself. Where is the code that has the bug? **It's in signup.js: form.addEventListener('keydown', e => { if (e.key === 'Enter') submitSignup(); }); form.addEventListener('submit', e => { e.preventDefault(); submitSignup(); })**
- What does the bug do? **The signup form submits twice when I press Enter**
- How should I confirm the bug is fixed? **Run existing tests (Recommended)**
- How far should the change go? **Minimal fix (Recommended)**

*Claude used Bash: Search for signup.js under the Code folder*

**Claude:**

I couldn't find `signup.js` on this machine (I searched `~/Code` and the empty working directory), so I haven't edited the file or run any tests. Here's the fix to apply.

**Cause:** When you press Enter in a text field, the browser submits the form on its own and fires the `submit` event. Your `keydown` handler calls `submitSignup()` on that same keypress, and then the `submit` handler calls it again, so it runs twice.

**Fix:** Delete the `keydown` listener. The `submit` handler already covers Enter, the submit button, and other ways of submitting.

```js
form.addEventListener('submit', e => {
  e.preventDefault();
  submitSignup();
});
```

If the form has no submit button and Enter seemed to do nothing without the `keydown` handler, add a `<button type="submit">` to the form. Don't keep the key handler as a workaround.

**Checking it:** Run the project's existing tests. Then type in a field, press Enter, and confirm that one signup request appears in the Network tab, not two.

Following your choice of a minimal fix, I didn't add a guard against double-clicking submit. If you want me to make the edit and run the tests myself, send me the file's path.


## 4. No request at all

With nothing to clarify, it asks what you want to do and waits.

**You:**

```
/clarify
```

**Claude:**

What would you like me to do?

