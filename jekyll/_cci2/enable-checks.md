---
layout: classic-docs
title: "Enabling GitHub Checks"
short-title: "Enabling GitHub Checks"
description: "How to enable GitHub Checks for CircleCI"
categories: [getting-started]
contentTags:
  platform:
  - Cloud
---

## Introduction
{: #introduction }

**The GitHub checks integration feature is not currently available on CircleCI server.**

This document describes how to enable the GitHub Checks feature and authorize CircleCI to report workflow status to the GitHub user interface.

GitHub Checks provides you with workflow status messages and gives the option to rerun workflows from the GitHub Checks page.

After GitHub Checks is enabled, CircleCI workflow status is reported under the checks tab on GitHub.

![CircleCI checks on GitHub]({{site.baseurl}}/assets/img/docs/checks_tab.png)

GitHub does not currently provide a granular way for you to rerun workflows. When you select the Re-run checks button, you will automatically re-run all checks, regardless of whether you selected "re-run failed checks" or "rerun all checks" from the Re-run checks button.
{: class="alert alert-info" }

## GitHub Check and GitHub status updates
{: #github-check-and-github-status-updates }

GitHub Checks should not be confused with GitHub status updates:

* GitHub Checks are administered from the GitHub UI, and are reported in the GitHub UI per _workflow_.
  * GitHub Checks from OAuth integrations have the same name as the associated workflow.
  * GitHub Checks from GitHub App integrations are named using a combination of the workflow name and the first eight characters of the pipeline definition ID. The identifier is used to prevent naming collisions. The pipeline definition ID is available in the **Project Settings** > **Pipelines** page in the CircleCI web app.
* GitHub status updates are the default way status updates from your builds are reported in the GitHub UI, and they are reported per _job_.

If both these features are enabled, in a GitHub PR view the Checks tab will show **workflow status** and the Checks section in the PR conversation view will show **job status**.

If you are using GitHuck Checks or GitHub Status updates with the [skip jobs feature](/docs/skip-build/#skip-jobs),
the status of the skipped builds will not be reported even though the checks will be created in GitHub.

## Prerequisites
{: #prerequisites }

- You must be using the cloud-hosted version of CircleCI.
- Your project must be using [Workflows](/docs/workflows/).
- You must be an admin on your GitHub repository to authorize installation of the CircleCI Checks integration.

## Enable GitHub Checks
{: #enable-github-checks }

1. In the CircleCI app sidebar, select **Organization Settings**.
2. Select **VCS**.
3. Select **Manage GitHub Checks**.
![CircleCI VCS Settings Page]({{site.baseurl}}/assets/img/docs/github-checks.png)
4. You are now in the GitHub CircleCI Checks App settings page. Select the repositories that should use checks and select **Save**.

After installation completes, the Checks tab on the GitHub PR view will be populated with workflow status information, and from here you can rerun workflows or navigate to the CircleCI app to view further information.

## Checks status reporting
{: #checks-status-reporting }

CircleCI reports the status of workflows and all corresponding jobs under the Checks tab on GitHub. Additionally, Checks provides a button to rerun all workflows configured for your project.

After a rerun is initiated, CircleCI reruns the workflows from the start and reports the status in the Checks tab. To navigate to the CircleCI app from GitHub, select the **View more details on CircleCI Checks** link.

Your project will continue receiving job level status after GitHub Checks is turned on. You can disable this in the GitHub Status updates section of the **Project Settings** > **Advanced Settings** page in the CircleCI app.
{: class="alert alert-info" }

## Disable GitHub Checks for a project
{: #disable-github-checks-for-a-project }

To disable the GitHub Checks integration, navigate to the **Organization Settings** page in the CircleCI app, then remove the repositories using GitHub Checks, as follows:

1. Select **Organization Settings** in the CircleCI sidebar.
2. Select **VCS**.
3. Select **Manage GitHub Checks**. The Update CircleCI Checks repository access page appears. ![CircleCI VCS Settings Page]({{site.baseurl}}/assets/img/docs/github-checks.png)
4. Deselect the repository to uninstall the Checks integration. Select **Save**. If you are removing GitHub checks from the only repo you have it installed on, you will need to either suspend or uninstall CircleCI Checks.
5. Confirm that GitHub status updates have been enabled on your project so you will see job level status within PRs in GitHub. To do this, go to the CircleCI app and navigate to **Project Settings** > **Advanced Settings** and confirm that the setting `GitHub Status Updates` is set to `on`. ![CircleCI VCS Settings Page]({{site.baseurl}}/assets/img/docs/github-status-updates.png)

## Uninstall Checks for the organization
{: #uninstall-checks-for-the-organization }

1. Select **Organization Settings** in the CircleCI sidebar.
2. Select **VCS**.
3. Select **Manage GitHub Checks**.![CircleCI VCS Settings Page]({{site.baseurl}}/assets/img/docs/github-checks.png)
4. Scroll down and select the Uninstall button to uninstall the GitHub Checks app.
5. Confirm that GitHub status updates have been enabled on your projects so you will see job level status within PRs in GitHub. To do this, go to the CircleCI app and navigate to **Project Settings** > **Advanced Settings** and confirm that the setting `GitHub Status Updates` is set to `on`.![CircleCI VCS Settings Page]({{site.baseurl}}/assets/img/docs/github-status-updates.png)

GitHub OAuth integrations will automatically re-enable job level status when the GitHub Checks integration is uninstalled. CircleCI GitHub App integrations will not disable or re-enable job level status automatically.
{: class="alert alert-info" }

## Troubleshoot GitHub Checks waiting for status in GitHub
{: #troubleshoot-github-checks-waiting-for-status-in-github }

`ci/circleci:build — Waiting for status to be reported`

If you have enabled GitHub Checks in your GitHub repository, but the status check never completes on GitHub Checks tab, there may be status settings in GitHub that you need to deselect. For example, if you choose to protect your branches, you may need to deselect the `ci/circleci:build` status key as this check refers to the job status from CircleCI, as follows:

![Uncheck GitHub Job Status Keys]({{site.baseurl}}/assets/img/docs/github_job_status.png)

Having the `ci/circleci:build` checkbox enabled will prevent the status from showing as completed in GitHub when using a GitHub Check because CircleCI posts statuses to GitHub at a workflow level rather than a job level.

Go to **Settings** > **Branches** in GitHub and select the **Edit** button on the protected branch to deselect the settings, for example `https://github.com/your-org/project/settings/branches`.

## Next steps
{: #next-steps }

- [Add an SSH key to CircleCI](/docs/add-ssh-key)