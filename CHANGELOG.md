# Change Log

Per-session record of work on this site. Newest entries at the bottom.

## 2026-08-10

**Activity:** Diagnosed and fixed a broken deploy, then rebuilt the homepage for the job market. GitHub Pages was on legacy deploy-from-branch, so the live site was GitHub's Jekyll render of `README.md` ("How to Edit Each Section"); the Hugo workflow had also been failing silently since Aug 3. Switched Pages to Actions and fixed the build. Restructured the layout to match jie-song.github.io: left sidebar (headshot, name, affiliation, email, CV) and top nav (Research / Teaching / CV). Added the renamed JMP with new title and abstract, three work-in-progress papers, a policy writing section, and an SC monogram favicon in Berkeley blue/gold. Drafted a LaTeX CV from the Word version and published it to the site. Cropped a headshot for applications.

**Decisions:**
- Overrode `layouts/partials/foot.html` instead of patching the theme, which is a submodule of someone else's repo. This is what unblocked the build: the theme called `_internal/google_analytics_async.html`, removed in Hugo 0.123, while the workflow pins 0.128.
- Built Teaching into `layouts/index.html` because the theme's `_default/list.html` and `single.html` are zero-byte files, so section pages render nothing.
- Filed the JMP under its own `data/job_market_paper/` section rather than reusing Working Papers.
- Removed the Working Papers section from the CV; this also dropped "Worker Preferences for Flexibility and the Persistence of Small Firms" entirely.
- Kept `static/pdf/Chandra_Shreya_UCB_FSPW_Paper.pdf` at its exact path and the repo public throughout, since a conference submission links to that GitHub blob URL.

**Files changed:** `layouts/index.html`, `layouts/partials/header.html`, `static/css/custom.css`, `static/favicon.svg`, `static/favicon.ico`, `static/apple-touch-icon.png` (all new); `config.toml`; `content/sections/aboutme.md`; deleted `content/sections/personal.md` and `.github/workflows/hugo.yaml`; `data/job_market_paper/list.yaml`, `data/work_in_progress/list.yaml`, `data/policy_writing/list.yaml`, `data/teaching/classes.yaml`; CV swapped to `static/pdf/Shreya_Chandra_CV_Aug26.pdf` and Sep25 removed. Outside the repo: `Dropbox/SC_Applications/2_CV/latex/Shreya_Chandra_CV.tex` and `.pdf`, `Dropbox/SC_Applications/Shreya_Chandra_headshot.jpg`, and a new `/update-paper` command.

**Next:**
1. Add back a Personal/Other section — `content/cookie.jpg` is still in the repo for it.
2. Decide whether "Worker Preferences for Flexibility and the Persistence of Small Firms" should return to the CV under Work in Progress.
3. Optional CV polish: trim to 2 pages, confirm Aprajit Mahajan's rank (the ARE directory lists Associate Professor), and consider moving References below the research sections.

## 2026-08-11

**Activity:** Looked into changing the site URL. No changes made — site stays at `shreyachaa.github.io` for now.

**Findings (as of 2026-08-11, re-verify before acting):**
- `shreyachandra.com` is unregistered (whois returns "No match"). Roughly $10–15/yr.
- GitHub username `shreyachandra` returns 404 from the API, so it is probably available; GitHub reserves some names, so only the rename page confirms. `shreya-chandra` is taken.
- A custom domain needs no Hugo config change: `config.toml` sets `relativeURLs = true` and `.github/workflows/hugo.yml:58` passes `--baseURL` from the Pages action.
- Getting `shreyachandra.github.io` would require renaming the GitHub *account* from `shreyachaa` plus the repo, since user sites must match the username. Risk: the conference submission that links to a `github.com/shreyachaa/...` blob URL relies on GitHub's rename redirect, which breaks permanently if anyone else claims `shreyachaa`.

**Decision:** Deferred. Custom domain is the recommended path (no renames, permanent, keeps existing links working), but the user wants to think about whether to buy it.

**Files changed:** none (this entry only).

**Steps if the domain is purchased later:**
1. Register `shreyachandra.com` at a registrar.
2. Add `static/CNAME` containing `shreyachandra.com`; commit and push to `source`.
3. Set the custom domain in repo Settings → Pages.
4. At the registrar: four apex `A` records → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`; `CNAME` for `www` → `shreyachaa.github.io`.
5. Wait for the certificate, then enable "Enforce HTTPS".
6. Check that CV and paper links still resolve.

## 2026-08-12

**Activity:** Took every PDF link off the site until the materials are final. The job market paper is now an unlinked title with "Draft coming soon." under it, keeping the "previously circulated as" note and the IGC blog post link; the abstract is gone. Both CV links (top nav and sidebar) are gone too.

**Decisions:**
- Added `layouts/partials/publication.html` as an override of the theme partial, adding a `status` field that renders directly below the title. Overriding rather than patching, since the theme is a submodule of someone else's repo. The field is generic, so any entry in any section can use it.
- Commented out `cvlink` in `config.toml` rather than deleting it. Both CV links are guarded by `{{ with .Site.Params.cvlink }}`, so one commented line removes both and uncommenting restores them.
- Left both PDFs in place. `static/pdf/Chandra_Shreya_UCB_FSPW_Paper.pdf` and `static/pdf/Shreya_Chandra_CV_Aug26.pdf` are still in the public repo and still reachable by direct URL — only the links are gone. The paper PDF must stay at that exact path for the conference submission that links to its GitHub blob URL (see 2026-08-10).

**Files changed:** `data/job_market_paper/list.yaml` (removed `pdflink` and `abstract`, added `status`), `layouts/partials/publication.html` (new), `config.toml`.

**Verified:** Local Hugo build (v0.152.2) renders the JMP title as plain text with no anchor, no abstract toggle, the status and note lines, and the IGC link. No CV link anywhere on the page. Policy Writing's external PDF link is untouched.

**Deployed:** Pushed to `source` as `1dc37fd`; the Pages workflow succeeded. Confirmed against the live page at `shreyachaa.github.io` — no paper or CV link in the HTML, "Draft coming soon." renders, note and IGC link intact.

**Gotcha:** this repo has an `upstream` remote pointing at `gautamrao/gautamrao.github.io`, the site this one was forked from, and no `gh` default repo is set. Plain `gh run list` therefore reports *Gautam Rao's* deploy history, which looks like this site has not deployed since July. Use `gh run list -R shreyachaa/shreyachaa.github.io`, or run `gh repo set-default` once.

**Known cosmetic side effect:** the sidebar email line carries `p.contactinfo` (10px bottom margin) and was previously followed by the CV line with `p.lastcontactinfo` (20px). The sidebar now ends 10px tighter. Not worth a second override to fix.

**Next:**
1. Restore both links when the draft and CV are final — uncomment `cvlink`, re-add `pdflink` to the JMP entry, drop `status`, and put the abstract back (the removed text is in this file's git history at `449059d..`).
2. Carried over from 2026-08-10: add back a Personal/Other section (`content/cookie.jpg` is still in the repo); decide whether "Worker Preferences for Flexibility and the Persistence of Small Firms" returns to the CV under Work in Progress; optional CV polish (trim to 2 pages, confirm Aprajit Mahajan's rank, consider moving References below the research sections).
3. Carried over from 2026-08-11: the six-step custom-domain setup, if `shreyachandra.com` gets bought.

## 2026-09-30

**Activity:** Published the current academic CV and restored the CV links, which had been hidden since 10 August. Edited the LaTeX source: added a `* Scheduled.` legend to the foot of Conferences & Workshops (five entries carried an unexplained asterisk), fixed `CEGA R\^2` to `CEGA R$^2$` (`\^` is a circumflex accent, not a superscript), and moved Conferences above Teaching so the order is now Conferences > Teaching > Research & Professional Experience.

**Decisions:**
- **Dropped the date suffix from the filename.** The PDF is now `static/pdf/Shreya_Chandra_CV.pdf`, not `_Sep26`, so future updates overwrite in place and no link ever goes stale.
- **Left `Shreya_Chandra_CV_Aug26.pdf` in the repo.** It is no longer linked but is still reachable by direct URL, so deleting it could break a link someone already holds. Remove it deliberately, not as cleanup.
- **The live CV source is `Dropbox/SC_Applications/Jobs/materials/cv/Academic/Shreya_Chandra_CV.tex`.** The path recorded in the 2026-08-10 entry above, `Dropbox/SC_Applications/2_CV/latex/`, was archived to `2_CV/z_archives/latex/` on 18 September and is 10 lines behind — it lacks the Working Papers section, the Pay Contracts entry and the current conference list. That entry is stale; do not edit the archived copy.

**Note on what this publish ships.** The live PDF dated from 10 August, so activating the link also exposes seven weeks of accumulated CV changes: a new Working Papers section, the Pay Contracts entry (*Piloting in progress*), Organizational Economics commented out of Fields, the author name added to the JMP title line, and Research & Professional Experience relocated to near the end. Three pages, unchanged.

**Files changed:** `config.toml` (`cvlink` uncommented and repointed), `static/pdf/Shreya_Chandra_CV.pdf` (new). Outside the repo: `Jobs/materials/cv/Academic/Shreya_Chandra_CV.tex` (backup at `.tex.bak-20260930`).

**Next:** Decide whether to move the domain to shreyachandra.com.
