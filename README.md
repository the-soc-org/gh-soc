# gh-soc: GitHub CLI extension to manage the School of Commonwealths

- init: Initializes the setup by creating necessary configuration files (user_config.sh, source_config.sh, system_config.sh) and an email file (emails.txt) with default or placeholder values. It prepares the environment for the script's execution.

- precheck: Validates the environment and configuration files to ensure all required dependencies, variables, and settings are correctly in place before executing main operations. It checks for GitHub CLI, Git, and necessary utilities (sed, grep, jq, date, touch) installations, and validates configuration values for compatibility and correctness.

- sync: Performs the synchronization of repositories from a source organization or user to a target GitHub organization. It includes forking source repositories, setting up repositories as private templates if necessary, updating repository contents without cloning, and setting up GitHub Pages and repository website links.

- open: Prepares the organization and its repositories for the course's commencement. It involves setting organization and team configurations, creating a team and a project from a template, linking the project to the team, inviting members to the team, cloning repositories, setting a team as codeowners, committing changes, and applying branch protection rules.

- log: Captures and logs the current state of a specified GitHub project into a JSON file for archival or analysis purposes. It requires the TARGET_PROJECT_TITLE to be defined in the user_config.sh file.

- close: Concludes the course by archiving project data and optionally deleting the team associated with the course. It ensures that all course data is preserved for future reference before cleaning up the organizational structure.

- reopen: Reopens a course by creating successor repositories from templates of the original repositories. It essentially duplicates the setup for a new iteration of the course, including updating repository contents, setting up GitHub Pages, and reapplying team and branch configurations.

- delete: Cleans up by deleting the organization and its associated data after archiving necessary information. It's a final step to remove the organization's presence and data from GitHub, ensuring a clean state.
