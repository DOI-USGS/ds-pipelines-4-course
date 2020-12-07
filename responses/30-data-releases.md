## Data Releases

* Connecting one pipeline to another: point to ../neighboring-repo/x_phase/out/yy for data to import. Consider using `I()` around that file name plus a `dummy='[date]'` argument to fully break the pipeline if you *don't* want rebuilds when that file changes (e.g., if the other pipeline is non-static and you're comfortable taking a snapshot with this project, e.g., mntoha-data-release is a snapshot of lake-temperature-model-prep)

* Use SB liberally for connecting one pipeline to the next. If you build an early target with reference to ../neighboring-repo/x_phase/out/yy, try to only build one target with that reference, then push the info to a shared-cache or git file or SB, then make all subsequent targets depend on that shared-cache version of the file. In this way there's only one point at which we're depending on that external file, and thus we only need one person to build from that external file when the time is right.

