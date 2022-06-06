# 4.2.4 Release

## Timeline:

  - 5 weeks cycle (Mon-Fri) - Start Date: May 30, 2022; End Date: Jul 01, 2022
    - First 4 weeks dev-focussed work
    - Last 1 week, only testing and bug-fixing

  - Within each week of the first 4 weeks:
    - First four days (Mon-Thu) dedicated to dev
    - The last day (Fri) dedicated for testing and bug-fixing

## Scope:
#### Restriction: No major new features or capabilities or enhancements

### External:

  - Python, .Net core and GoLang support
  - Support for GitHub, gitlab & bitbucket SSO for community edition
  - Fixing only those bugs (known, internally found or reported by users) within the scope:
    - Ensure gopaddle Community (Lite) and Enterpise (Onprem) editions are fully usable and all features work correctly as advertised
      - All artificial restrictions and limitations imposed shall be addressed, provided they are within the scope of features advertised

### Internal:
  - Automation: Create a Jenkins pipeline to install/provision gopaddle Community (Lite) & Enterprise (Onprem) editions and initiate the regression (Gayathri)
  - Dockerfile based installation of Beta server (Logesh)
  - Direct discovery of resources (Renu)
  - Documentation: Write architecture and design documents for each of the modules owned by the engineers. We'll do this on the gopaddle internal wiki. Shall also include:
    - Build process
    - Branching strategies
    - Installer
    - gpctl
    - sail
  - Code Maintainability: Code cleanup, including:
    - formatting logs
    - cleaning up unwanted logs
    - adding logs wherever necessary (especially error logging)
      - Note from Vinothini:  We did a cycle to format the logs in such a way they can be read in elastisearch, but over a period of time, we started breaking the format. So we must bring back the log formats.
  - Code stability: Each of the engineers to start writing newer tests related to their modules to be included and run as part of Automation

### Development model:

  - There will be a 'dev' branch for each code repository. Engineers shall fork from this 'dev' branch and submit Pull requests to be merged back to 'dev' branch
  - Before the release, 'dev' branch will be merged with main branch, tagged with the Release number. As soon as the release is done, the dev branch will be deleted

### Standup meetings:

  - Twice a day, at 10 AM, and at 6 PM
  - It will be at most for 20 minutes only. No one will be allowed to hijack it beyond that time
  - It will be purely based on going through Jira issues

For this to work well, everybody needs to update Jira better:

  - the state should always indicate what are they actively working on make the Jira issue more readable
  - in the case of bugs, fill all the relevant details and attachments to make it self-contained and self-explanatory
    - (Today, you will learn very less about any issue from Jira, whereas it can be a gold-mine to learn if issues have proper details enclosed about problems)

### Daily Session: "Get to know how this works"
  - Daily 1 PM to 1.30 PM

### To Be Evaluated for inclusion in scope:

  - Quick start wizard for first time user - Advanced version
  - direct discovery of resources
  - support for OpenShift and VMware Tanzu platforms
