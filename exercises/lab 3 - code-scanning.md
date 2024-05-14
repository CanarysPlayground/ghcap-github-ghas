## Code scanning

Code scanning enables developers to integrate security analysis tooling into their developing workflow. In this exercise, we will focus on the CodeQL static analysis tooling that helps developers detect common vulnerabilities and coding errors.

### Contents

- [Enabling code scanning](#enabling-code-scanning)
- [Reviewing any failed analysis job](#reviewing-any-failed-analysis-job)
- [Using context and expressions to modify build](#using-context-and-expressions-to-modify-build)
- [Reviewing and managing results](#reviewing-and-managing-results)
- [Triaging a result in a PR](#triaging-a-result-in-a-pr)


### _**Exercise 3**_

#### Task 1: Enabling code scanning

#### Code Scanning: What is it?

Code scanning is a feature that you use to analyze the code in a GitHub repository to find security vulnerabilities and coding errors. Any problems identified by the analysis are shown in GitHub.

1. To enable GitHub Code Scanning, please navigate to the Settings of your repository, click Security & analysis click the Set up drop down for the Code Scanning feature and choose "Advanced" for this lab in order to generate the CodeQL related workflow file.

3. Review the created Action workflow file `codeql-analysis.yml` and choose `Start commit` to accept the default proposed workflow.

4. Head over to the `Actions` tab to see the created workflow in action. Click on the workflow to view details and status for each analysis job.


#### Task 2: Reviewing any failed analysis job

CodeQL requires a build of compiled languages. An analysis job can fail if our *autobuilder* is unable to build a program to extract an analysis database.

1. Inside the workflow you'll see a list of jobs on the left. Click on the Java job to view the logging output and review any errors to determine if there's a build failure.

2. The `autobuild` compilation failure appears to be caused by a JDK version mismatch. Our project targets JDK version 15. How can we check the Java version that the GitHub hosted runner is using? Does the logging output provide any helpful information?

    <details>
    <summary>Solution</summary>

    - GitHub saves workflow files in the `.github/workflows` directory of your repository. You can add a command to the existing `codeql-analysis.yml` workflow to output the Java version.  Add this anywhere before the step that is failing to help in your debugging:

    ```yaml
    - run: |
        echo "java version"
        java -version
    ```

    </details>

3. The previous debugging has concluded that we have a mismatch.  Resolve the JDK version issue by using the `setup-java` Action in `codeql-analysis.yml` to explicitly specify a version.  This should be added to the workflow before the `autobuild` step to properly configure the runtime environment before the build.

    <details>
    <summary>Solution</summary>

        uses: actions/setup-java@v3
        with:
            java-version: 16
            distribution: 'microsoft'

    </details>


#### Task 3: Using context and expressions to modify build

How would you [modify](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions) the workflow such that the autobuild step only targets compiled languages (`java` in our repository)?

  <details>
  <summary>Solution</summary>

  You can run this step for only `Java` analysis when you use the `if` expression and `matrix` context.

  ```yaml
  - if: matrix.language == 'java'  
    uses: github/codeql-action/autobuild@v2
  ```
  </details>

#### Task 4: Reviewing and managing results

1. On the `Security` tab, view the `Code scanning alerts`.

2. For a result, determine:
    1. The issue reported.
    1. The corresponding query id.
    1. Its `Common Weakness Enumeration` identifier.
    1. The recommendation to solve the issue.
    1. The path from the `source` to the `sink`. Where would you apply a fix?
    1. Is it a *true positive* or *false positive*?

#### Task 5: Triaging a result in a PR

The default workflow configuration enables code scanning on PRs.
Follow the next steps to see it in action.

1. Add a vulnerable snippet of code and commit it to a patch branch and create a PR.

    Make the following change in `frontend/src/components/AuthorizationCallback.vue:27`

    ```javascript
     - if (this.hasCode && this.hasState) {
     + eval(this.code)    
     + if (this.hasCode && this.hasState) {
    ```
2. Is the vulnerability detected in your PR?

3. You can also configure the check failures for code scanning. Go into the `Code security and analysis` settings and modify the Check Failures. Set it to `Only critical/ Only errors` and see how that affects the code scanning status check for subsequent PR checks. In the next steps, you will be enabling additional query suites that have other severity types.

