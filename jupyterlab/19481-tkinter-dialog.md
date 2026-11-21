# #19481 — tkinter file dialog opens behind the browser (repro + findings)

**Status:** Open, no PR. Findings comment posted 2026-11-21 (id 7057401138), verified live; awaiting a maintainer reply on whether they want a docs note or the issue moved to ipykernel.

## The issue

`tkinter.filedialog.askdirectory()` opens behind the browser window on Windows 11 instead of in front of it. The filer, nicolas-joffre, reported it on JupyterLab 4.6.3 in the browser and confirmed it also reproduces in JupyterLab Desktop 4.6.3-1. The dialog is only reachable with Alt+Tab or by minimising the browser.

## What was done

Kumar commented first (2026-11-20, id 7036001483), offering a reproduction on his Windows 11 laptop and asking for the filer's version and setup. The filer replied with the 4.6.3 confirmation, and Kumar ran the repro on 2026-11-21 — Windows 11 Build 26300, JupyterLab 4.7.0b2, Python 3.12.16, Chrome 157:

| Test | Result |
| --- | --- |
| Plain `askdirectory()` | Behind the browser — reproduced as reported |
| Topmost Tk root first (`root.attributes('-topmost', True)`) | In front |
| `%matplotlib tk` + `plt.plot()` first | In front, matching the filer's observation |

Before any code was written, the issue was checked properly: still open, no PR, nothing fixed on main — and JupyterLab's own code never touches tkinter (zero hits in the whole repo). The dialog is made by the kernel process, and Windows foreground-lock rules won't let a background process bring a window forward. Spyder hit the exact same bug (#26208) and never fixed it; their maintainer said the same thing — it is the kernel setup, not the frontend.

So there is probably no code fix in JupyterLab at all. The findings comment posted the repro results and both workarounds, and asked the maintainers whether they want a docs note or the issue moved to ipykernel.

## What we learned

- Check where the bug lives before writing any code. A repo can contain zero lines of the failing code — here, not one tkinter call in all of JupyterLab — and a fix in the wrong repo is worse than no fix.
- A repro with results is a contribution. The plain call, the topmost workaround, and the matplotlib-first workaround each say something different about the cause, and only running all three settles it.
- A comment that asks the maintainers which way to go is the right move on a `status:Needs Triage` issue. Guessing the fix direction first would have meant building a docs note nobody asked for, or a PR in a repo that cannot fix the bug.
- The same bug in another project is evidence, not a distraction. Spyder #26208, same filer, same symptom, same maintainer answer, saved a lot of wasted digging.

## Links

- Issue: https://github.com/jupyterlab/jupyterlab/issues/19481
- Spyder, same bug: https://github.com/spyder-ide/spyder/issues/26208
- Findings comment: https://github.com/jupyterlab/jupyterlab/issues/19481#issuecomment-7057401138
