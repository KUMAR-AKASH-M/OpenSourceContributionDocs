# #18830 — Open in Terminal fails on Windows with square brackets (not a JupyterLab bug)

**Status:** Closed as a finding, no PR. Kumar's findings comment was posted 2026-11-13 (comment id 6850089024, verified live). The issue stays open on GitHub; there is nothing to fix in JupyterLab.

## The issue

"Open in Terminal" on a folder whose name contains `[` `]` opens PowerShell in `C:\Windows\System32\WindowsPowerShell\v1.0` instead of the folder. Filed by raffaelemancuso 2026-05-03, labels `bug`, `pkg:terminal`, `os:windows`. The presumed cause in the issue body was that PowerShell treats square brackets as wildcards — but nobody had proved where the path actually breaks, and krassowski's open question from 2026-06-10 was whether the fix belonged in JupyterLab or in a jupyter-server extension. He has no Windows machine to test on.

## What was done

The full chain was traced first: JupyterLab sends the folder path verbatim as JSON to POST /api/terminals; the `jupyter_server_terminals` handler resolves it with pathlib only; terminado and pywinpty pass `cwd` verbatim to the process spawn. No bracket handling exists anywhere in that chain, so the break had to be downstream — in the native pty layer or in PowerShell itself.

Then it was reproduced on Kumar's Windows laptop (JupyterLab 4.7.0b1, jupyter_server_terminals 0.5.4, terminado 0.18.1, pywinpty 3.1.2, Windows PowerShell 5.1.26100.9818), with a control test that split the blame: plain Python `subprocess` with the bracket folder as cwd also landed PowerShell in System32 — yet under the same cwd, `cmd.exe /c cd` reported the bracket folder, and `[System.Environment]::CurrentDirectory` inside PowerShell was the bracket folder too. The same split held under pywinpty, and the JupyterLab UI repro landed in System32 as well.

So the working directory arrives correct all the way down, and PowerShell 5.1 itself mangles it at startup: its location provider interprets `[` `]` as wildcards when resolving the initial location and falls back to $PSHOME. The findings comment records that proof and answers krassowski's question — the fix belongs in neither repo. No screenshot or video was added: the issue already has screenshots of the same symptom, and the comment's value is the text proof. Whether PowerShell 7 handles it is untested — `pwsh` is not installed on Kumar's laptop.

## What we learned

- A guess on record is not proof. The issue body already suggested PowerShell, and a commenter had called it a PowerShell bug in May — but without a repro and a control test, the fix-location question stayed open for five months.
- Split the blame with a control test before designing any fix. One plain-subprocess run, with `cmd.exe` as the control, proved the cwd was correct and the shell was at fault — that single comparison is what made "no PR" a conclusion instead of a guess.
- A negative result is still a contribution when it answers the maintainer's open question and stops the next person re-tracing the same chain. Posting the finding, with the versions and the control results, is the work.
- Windows-only bugs need a Windows machine. krassowski could not test this one; Kumar could, and that was the whole reason this issue fit him.

## Links

- Issue: https://github.com/jupyterlab/jupyterlab/issues/18830
- Findings comment: https://github.com/jupyterlab/jupyterlab/issues/18830#issuecomment-6850089024
