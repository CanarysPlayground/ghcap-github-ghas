## Dependabot

The hands-on lab has the goal to learn you how to enable the Dependency and Dependabot features and gain hands-on experience with Dependatbot's ability to update your dependencies automatically.
### Contents

- [Enabling Dependabot alerts](#enabling-dependabot-alerts)
- [Reviewing the dependency graph](#reviewing-the-dependency-graph)
- [Viewing and managing results](#viewing-and-managing-results)
- [Enabling Dependabot security updates](#enabling-dependabot-security-updates)
- [Configuring Dependabot security updates](#configuring-dependabot-security-updates)

### _**Exercise 1**_

#### Task 1: Enabling Dependabot alerts
Dependabot can be enabled in the settings of an organization or a repository.

- Go to the repository settings and enable Dependabot alerts in the `Code security and analysis` section. You will be prompted to enable the dependency graph if it's not enabled already.

#### Task 2: Reviewing the dependency graph
Dependabot uses the dependency graph to determine which dependencies are used by your project.

- Verify in the dependency graph that it found dependencies for:
    - The frontend service.
    - The authentication service.
    - The gallery service.
    - The storage service.

The dependency graph can be accessed from the `Insights` tab in your repository.

#### Task 3: Viewing and managing results

After a few minutes, the `Security` tab in the repository will indicate that there are new security alerts. You will see a **Create a security update** button; click this button to create a pull request (PR) to update the vulnerable dependency. The next section will show you how to enable security updates for all applicable Dependabot alerts.

**Note**: If this not the case, we can trigger an analysis by updating `authn-service/requirements.txt`

1. Go to the `Dependabot alerts` section to view the detected dependency issues.

    For each dependency alert, we have the option to create a security update or to dismiss the alert with a reason.

1.  For one of the alerts, `create a dependency security update`. If Dependabot can update the dependency automatically, it will create a PR.

1. For one of the alerts, `dimiss` the alert.

#### Task 4: Enabling Dependabot security updates

Dependabot can automatically create PRs to upgrade vulnerable dependencies to non-vulnerable versions. Please note that there may be some Dependabot alerts that don't have patches. In those cases, a security update is not available.

- Go to the `repository settings` and enable `Dependabot security updates` in the `Code security & analysis` section.

After a few minutes multiple PRs will be created that will upgrade vulnerable dependencies.

#### Task 5: Configuring Dependabot version updates

You can enable Dependabot [*version updates*](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/enabling-and-disabling-version-updates)  by committing a dependabot.yml configuration file to your repository. If you enable the feature in your settings page, GitHub creates a basic file which you can edit, otherwise you can create the file using any file editor.


  1. Navigate to the main page of the repository.

  1. Go to the repository settings and enable Dependabot Version updates in the `Code security and analysis` section.

  2. It will open a basic dependabot.yml configuration file in the .github directory of your repository.

  3. Copy the following to dependabot.yml

        ```yaml
      version: 2
      updates:
        - package-ecosystem: "pip"
          directory: "/authn-service"
          schedule:
            interval: "daily"
          labels:
            - "triage-required"
          commit-message:
            prefix: "pip"
      ```

  4. After a few minutes multiple PRs will be created with version updates.

4. Use Dependabot chat commands to [manage PRs from Dependabot](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/managing-pull-requests-for-dependency-updates).



#### Task 6: Working with Dependency Review

If a PR has dependency changes, you can [review](https://docs.github.com/en/github/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/reviewing-dependency-changes-in-a-pull-request) them and see if there are known vulnerabilities with the dependency changes.

   1. Add a vulnerable dependency to `auth-service/requirements.txt` and commit to a new branch. For example, here's a vulnerable dependency:

    ```requirements.txt
    ...
    django-piston==0.2.0
    ```
   2. Create a PR, and click on `Files changed`.
   3. Click on the `Display the rich diff` button on the `requirements.txt` file to review dependency changes.

💡**Now that we're familiar with Dependabot, let's head over to the secret scanning section, and learn more about it! [Click here](lab%202%20-%20secret-scanning.md).** 💡
