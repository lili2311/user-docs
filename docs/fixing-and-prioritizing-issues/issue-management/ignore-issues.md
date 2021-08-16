# Ignore issues

If you do not want to fix a vulnerability or license issue, and don't want to see that issue in scan results, Snyk allows you to ignore it, either temporarily or permanently.

Issues can be ignored and viewed via the snyk.io UI, the Snyk APIs, the Snyk CLI and using the .snyk file.

Ignoring security issues should not be the default action, but it is sometimes necessary. The best practice is to fix or patch vulnerabilities, or to remove the vulnerable dependency, but there may still be reasons why you would want to suppress an issue – for example, if an issue doesn’t currently have a fix, you might want to ignore it until it does.

Some issues are irrelevant for certain projects \(e.g. a DDOS attack for an internal service\). Other times, an issue has a path that makes it non-exploitable. In these scenarios, you should still fix the issue, as although the vulnerability is not exploitable today, it may be exploitable in future.

### Ignoring issues in the UI

Each issue card has an **Ignore** button that opens up a dialog where you can select why you want to ignore the issue, and how long to ignore it.

If you select **Ignore temporarily,** then you can check the **Until fix is available** checkbox:

This will resurface the vulnerability as soon as we have a fix for it, and you can optionally give additional details on why you’re ignoring the issue. This is checked by default if there is currently no remediation available for this issue.

An issue is ignored until ANY of the conditions happen - either the ignore period expires, OR the vuln becomes fixable.

When you ignore an issue in our UI, it will show who ignored it and allow you to edit or unignore it.

### Ignoring issues in the CLI

Suppressing issues is possible via the CLI. For node.js projects, you can use **Snyk wizard**, which will give you the option of ignoring the vulnerability for a period of 30 days. For other supported languages,  or to specify a different duration, use **snyk ignore**.

`snyk ignore --id='npm:braces:20180219' --expiry='2018-04-01' --reason='testing'`

See [Ignore vulnerabilities using Snyk CLI](https://support.snyk.io/hc/en-us/articles/360003851317-Ignore-vulnerabilities) for more details.

When using **Snyk wizard** or **Snyk ignore**, the .snyk policy file is updated with the path and given a reason \(if one was provided\). For example:

```text
'npm:moment:20170905':
- moment:
reason: The reason given
expires: '2017-12-29T16:10:16.946Z'
```

### Scanning from the CLI or CI/CD, Ignoring in the UI

Ignores between a CLI \(or CI/CD run\) and the Snyk UI are synchronized. So the flow is:

1. A project is scanned and pushed to the UI using **snyk monitor**.
2. You see the results of the scan and choose to ignore an issue.
3. The issue is ignored when running **snyk test** or **snyk monitor** in the CI/CD or CLI

For example:

**snyk test** before ignoring in the UI:

**snyk test** after ignoring in the UI:

It is important that the above is true if you ignore the project imported by **snyk monitor** from the CLI or CI/CD.

The same repo imported from the SCM is considered as a different project, and any ignore on an SCM project does not impact the results of a **snyk test** from a CLI or CI/CD. SCM and CI project behave as two stand alone projects.

### Ignoring issues with the .snyk file

For all projects, you can ignore the vulnerability by creating a `.snyk` YAML file.

![null](https://uploads.intercomcdn.com/i/o/23911244/1eeea9dbbaa687703958ce9e/Screen+Shot+2017-05-10+at+11.16.57+AM.png)

For example, if you wanted to ignore the vulnerability with SNYK ID [SNYK-RUBY-FASTREADER-20085](https://snyk.io/vuln/SNYK-RUBY-FASTREADER-20085) in `fastreader`, with the reason “No remediation available” until 01 Jan 2017, you would write:

![null](https://uploads.intercomcdn.com/i/o/23911290/a6da5a57db28311a01a1d43e/Screen+Shot+2017-05-10+at+11.17.26+AM.png)

See [The .snyk file](https://docs.snyk.io/fixing-and-prioritizing-issues/policies/the-.snyk-file) for more details.

### Set who can configure ignore settings

Since suppressing vulnerabilities carries a level of risk, you can make this available to admins only: go to your organization settings &gt; **General**, and select **Admin users only** in the **Ignores** section \(this also disables ignores from being added via the CLI\).

You can also choose to set the more details field to be a compulsory field when an issue is being ignored, requiring the user to enter a reason for each ignore.

If you have access to our Reports feature, you will also be able to see an overview of how many issues in your organization’s projects are ignored, along with an option to filter these so you can drill down into each one. If the issue was ignored in our UI, we include a credit for additional accountability, so you can see who initiated it.

See [Reports](https://docs.snyk.io/reports-1) for more details.

### Ignoring issues in Snyk Code

For [Snyk Code](https://docs.snyk.io/snyk-code), ignore functionality may capture a wider range of issues than other products.

Snyk Code's static code analysis transforms the input code into an **intermediate representation**, which captures the flow of code, but abstracts away some details. Snyk Code uses this representation to recognize the same issue even when you refactor your code or rename a variable.

So when you ignore an issue, Snyk Code can also ignore that issue if it occurs in multiple places in your code, even with minor code changes. This avoids seeing multiple duplicate reports for pieces of code with the same ignored issue.

As an example, the following two code snippets \(despite textual differences\) denounce the same issue, as we only renamed the variables:

```text
var fs = require('fs');
var logFileName = req.query.file || 'standard_log.log';
var logfile = fs.readFile(logFileName, "utf8", function(err, data) {...
```

```text
var filesystem = require('fs');
var generalLogFileName = req.query.file || 'standard_log.log'; 
var handleLogFile = filesystem.readFile(generalLogFileName, "utf8", function(err, data) {...
```

See [Using Snyk Code \(web\)](https://support.snyk.io/hc/en-us/articles/360017147558#Ignore)  for more details of using the Ignore function for Snyk Code.
