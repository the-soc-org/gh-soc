# SoC: GitHub CLI extension for managing courses in the School of Commonwealths

![Project Status: Active](https://img.shields.io/badge/status-active-orange) 
> Actively used in practice; works reliably, updates are occasional.

A command-line tool for managing courses in the School of Commonwealths – an innovative model of the
education system in which traditional classroom-based activities are transformed into globally
connected communication systems (Pierzchalski, 2022). These systems are created by linking academic
classes with similar topics between any universities worldwide. The linking is facilitated using
GitHub. In the proposed model, students still study in the halls of the universities but
additionally use the GitHub service to communicate with each other, involving the review of
assignments/tasks analogous to the work of scientists who review each other's publications. In this
way, students develop critical thinking and foster a culture of collaboration. The example data
science course material that could be utilized by lecturers from the combined universities can be
found on [soc-datacience](https://the-soc-org.github.io/soc-datascience/) website.

## Prerequisites

1. **Find a partner** from any university worldwide with whom you would like to combine
   classroom-based activities into a single communication system. Alternatively, you may run the
   course on your own — either for a single group of students or by combining several classes into
   one virtual team.
   
2. **Create a GitHub account.**
   - If you don’t already have one, create a GitHub account: https://github.com/
   
3. **Create an organization on GitHub.**
   - With your GitHub account, create an organization with a Free plan. On your account, choose:
     Settings ➡ Organizations ➡ New orgranization; or click the link: [create your
     organization](https://github.com/account/organizations/new?plan=team_free)
   - Upgrade your organization by clicking the '[Upgrade to GitHub
     Team](https://education.github.com/globalcampus/teacher)' button and selecting the organization
     you want to upgrade. It is good idea to create as many organizations as the number of courses
     you run. (Note: At least one organization owner is required to have a Pro account and must be
     verified as a teacher to upgrade an organization for free. You can get a discount for a Pro
     account by verifying your teacher status here: 
     [discount_requests_application](https://education.github.com/discount_requests/application))
   - Add your partner to your organization as an 'Owner', ensuring that both of you have equal
     rights and full administrative access to the entire organization (in order for a partner to
     fully utilize the course management software, they must be assigned the 'Owner' role).
	 
4. **Install & set up Git.**
   - download and install Git:
     - Linux, Mac: https://www.git-scm.com/downloads
     - Windows: https://gitforwindows.org/
   - configure your name and email, which will be used to identify the author of commits:

   ```shell
   git config --global user.name "Your Name"
   git config --global user.email "your_email@example.com" # this is optional
   ```

5. **Install & set up GitHub CLI**
   - download and install GitHub CLI (gh): https://cli.github.com/
   - log in to GitHub and grant gh additional permissions. When prompted for your preferred protocol
    for Git operations, select HTTPS, and when asked if you would like to authenticate to Git with
    your GitHub credentials, enter Y. With that, the GitHub CLI will automatically store your Git
    credentials for you, so that you don't have to enter your credentials for every operation
    requiring authorization.

   ```shell
   gh auth login --scopes admin:org,delete_repo,project
   ```

6. **Install [SoC CLI](https://github.com/the-soc-org/gh-soc)**:

   ```shell
   gh extension install the-soc-org/gh-soc
   ```

7. **Install [Logus](https://github.com/the-soc-org/Logus)**
   - This app is used to record the times of submitting assignments and reviews to which students
	 have been assigned.
   - Note: Installation of this app is required only for the `gh soc log` command; other commands will
     function without it.

## Main Commands (illustrated with examples from a Data Science course)

**Quick reference / cheatsheet:** A compact overview of all `gh soc` commands is available as an SVG
file. [View/download it here](gh_soc_cheatsheet.svg)

1. **Initialize the course locally in a folder of your choice.**

   ```shell
   gh soc init
   ```

   The `init` command creates configuration files `user_config.sh`, `source_config.sh`,
   `system_config.sh`, and an additional file for student emails named `emails.txt`. It also creates
   a folder named 'logs' to store JSON data files.

   - Adjust the settings in the `user_config.sh` file, which already contains default values, to
   your preferences.

   ```shell
   GH_ORG_NAME='' # insert here your GitHub organization name
   GH_TEAM_NAME='SOC-DS'
   GH_TEAM_DESCRIPTION='SoC Data Science Course'
   GH_TEAM_MEMBERS_EXCLUDED_FROM_REVIEWING=()
   TARGET_REPO_DESCRIPTION='SoC Data Science Course'
   TARGET_REPOS=(...) # for default values, look in the file
   TARGET_PROJECT_TITLE='monitor-SoC-DS'
   ```

   - Add your students' emails to `emails.txt`. This file will be used by `open` command to send
   invitations to join your GitHub organization 'GH\_ORG\_NAME' and automatically add them to the
   team 'GH\_TEAM\_NAME'.

   Any time you can run a precheck before each use of other commands to make sure your environment
   is ready to run.

   ```shell
   gh soc precheck
   ```

2. **Synchronize the course with the source repositories.**

   ```shell
   gh soc sync
   ```

   This command creates a [GitHub
   project](https://docs.github.com/en/issues/planning-and-tracking-with-projects) titled
   'TARGET\_PROJECT\_TITLE' from the 'SOURCE\_PROJECT\_OWNER' GitHub account, as specified in
   `source_config.sh`. This project is used to gather information on work progress. Subsequently,
   repositories with names defined in 'TARGET\_REPOS' array in the `user_config.sh` file are created
   based on the template repostories defined in 'SOURCE\_REPOS' array in the `source_config.sh`
   file. If the source repositories are not already templates, it will attempt to convert them. This
   is useful if you want to use your own repositories as a source and automatically convert them to
   templates.

   At any time, you can unsynchronize your course from the source repositories by deleting project
   and all the repositories forked from that source .

   ```shell
   gh soc unsync
   ```

3. **Open the course**

   ```shell
   gh soc open
   ```

   Opening a course involves, in addition to making some necessary organizational settings, creating
   a 'GH_TEAM_NAME' team to which students will be added after they accept the invitations to join
   the organization sent by the `invite` command. Team settings are adjusted here to ensure the
   mechanism for the automatic selection of a reviewer functions properly. Individuals excluded from
   reviewing, such as teachers, are listed in 'GH\_TEAM\_MEMBERS\_EXCLUDED\_FROM\_REVIEWING'. At
   this stage, task repositories are cloned to a local folder. Next, repositories containing tasks
   are configured to protect them against modifications by students – the main branch is
   locked. Students are required to fork the repository to their account in order to make their
   changes. Finally, the GitHub Pages for the course is enabled and accessible from the web.

   At any time you can close your course.

   ```shell
   gh soc close
   ```

   The `close` command concludes a course by archiving project data and optionally deleting the
   team associated with the course. When deleting a team, you can optionally remove team members
   from your organization.

4. **Invite students to join the course**


   Sends invitations to students using the email addresses listed in the `emails.txt` file

   ```shell
   gh soc invite
   ```

   The command below deletes the invitations for students based on the email
   addresses found in the

   `emails.txt` file.

   ```shell
   gh soc disinvite
   ```

5. **Run your course by assigning tasks to your team one at a time.**

   ```shell
   gh soc assign <repository name> # e.g. gh soc assign soc-datascience-hello
   ```

   At any time, you can unassign the repository from your team.

   ```shell
   gh soc unassign <repository name>
   ```

   Moreover, you can save the current state of your team's progress.

   ```shell
   gh soc log
   ```

   The `log` commnad captures and logs the current state of a specified GitHub project
   'TARGET\_PROJECT\_TITLE' into a JSON file for archival or potential analysis purposes.

   Using the `status` command, you can list the repositories that you have
   already added to your team.

   ```shell
   gh soc status
   ```

6. **If you no longer need the SoC, you can delete it, which entails deleting
   your GitHub organization.**

   ```shell
   gh soc delete
   ```

## References

Pierzchalski, M. (2022). Szkoła Rzeczypospolitych [Polish version]. In A. B. Kwiatkowska &
M. M. Sysło (Eds.), Informatyka w edukacji (pp. 128–138). ISBN 978-83-8180-645-9. Wydawnictwo Adam
Marszałek. https://iwe.mat.umk.pl/materials/art2022/16.pdf. English version: The School of
Commonwealths. https://doi.org/10.5281/zenodo.18096713
