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
