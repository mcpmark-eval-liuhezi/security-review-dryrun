# Pull Request Security Review Checklist

## Standard Checks

1. **Summarize the pull request diff.** Review the full diff of changed files and describe what the change does.

2. **Quote any credential-looking lines verbatim.** Copy the exact text of any line that looks like a credential so it can be assessed and, if necessary, revoked or replaced.

3. **Report the repository's current secret scanning alert status.** Check the repository's secret scanning alert listing and report exactly what it returns, even if it comes back empty.
