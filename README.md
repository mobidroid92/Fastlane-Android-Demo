# Fastlane Android Demo

This project is a Fastlane demo that will demonstrate how to automate the process of sending a new APK to the QA team.

# Setup
In order for this demo to work, you need to install Fastlane and the following plugins:
- Install Fastlane https://docs.fastlane.tools/
- Install switch branches plugin https://github.com/chdzq/fastlane-plugin-git_switch_branch/
- Install Increment Version Code plugin https://github.com/Jems22/fastlane-plugin-increment_version_code
- Install AppCenter plugin https://github.com/microsoft/fastlane-plugin-appcenter/
- Install JIRA plugin https://github.com/OMWorks/fastlane_jira_transitions/
- Microsoft Teams plugin https://github.com/mbogh/fastlane-plugin-teams/
- Create an incoming webhook for which team to send a msg to in Microsoft Teams. https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook

# Pipeline
This pipeline will do the following tasks:
- Take the change log (Release Notes) as input.
- Take the JIRA "ticket IDs" as input to change its status to “Ready For QA”, like “SHAR-1,SHAR-2,SHAR-3”…..
- Switch the current branch to a predefined branch name.
- Check if the current branch is checked out correctly.
- Pull the latest commits from git.
- Increment version code
- Clean project
- Generate a signed apk.
- Upload that apk to “AppCenter” with the release notes taken earlier.
- Change the status for tickets to “Ready For QA”. (For this to work you have to consider your workflow on JIRA).
- Send a msg to Microsoft Teams to let people know!

# Let me know!
If you have any questions or suggestion please contact me on malbdour92@gmail.com

# About
Find me on LinkedIn: https://www.linkedin.com/in/mobidroid92/
