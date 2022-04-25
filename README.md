# jira-to-github-issues

This document explains step-by-step using https://github.com/lemeurherve/jira-issues-importer repository checkout to
migrate issues from a Jira issue Tracker to a Github repository Issues. 

1. Read the README at https://github.com/lemeurherve/jira-issues-importer
2. git clone https://github.com/lemeurherve/jira-issues-importer
3. Export all issues (include open/resolved/closed/etc). as XML.
4. If not already, create a Github account for testing of importing issues - this is important, get it right in testing first! Whatever name you call this account will be used on the tickets during import.
5. Create a PAT with repo:status and public_repo scope, limit your PAT to expire 30 days.
6. Create a new test repository, e.g. 'jira-to-github-issues'
7. Ensure 'issues' tab is enabled.
8. Edit importer.py (as at time of writing) and add these lines after line 178:

(after this line:         issue['key'] = jiraKey)

        jira_gh = f"{jiraKey}:{gh_issue_id}\n"
        with open('jira-keys-to-github-id.txt', 'a') as f:
          f.write(jira_gh)

6. Run `python3 main.py` and answer the questions!
7. Import failed? - Fix the problems! (could be you need to remove a problematic formatted issue in the XML. If so, remember to create that manually later.)
8. Import went well? Time to do it on the proper repository. First. give your Github User for importing appropriate rights on this repository. I found that I needed to give 'admin' access for the issues to be imported correctly (setting of labels etc.)
9. Ensure your user accepts the Invite
10. Ensure 'Issues' tab is enabled on the target repository.
11. delete the auto-created `jira-keys-to-github-id.txt` so it is clean for the import proper to populate.
12. Now run `python3 main.py` again.
13. Demote your Github user to 'read' or just remove altogether from your target repo.

Below is optional but recommended - Add a comment to all Jira tickets.

14. Edit jira-commenter.sh to suit - Ideally copy this outside of the repository, and copy the generated `jira-keys-to-github-id.txt` along with it.
15. Run `./jira-commenter.sh` - hope all goes well!
16. Close all your affected Jira Issues that are still open.
17. Ask your Jira Admin to remove your component from the Jira instance where the issues were imported from.
