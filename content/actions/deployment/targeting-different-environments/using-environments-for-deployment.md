---deploymTouch and hold a clip to pin it. Unpinned clips will be deleted after 1 hour.Touch and hold a clip to pin it. Unpinned clips will be deleted after 1 hour.ent target like production, staging, or development. When a GitHub ActionsGitHub Actions/Deployment/Target different environments/Use environments for deployment
Using environments for deployment
You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.

Who can use this feature?
Environments, environment secrets, and deployment protection rules are available in public repositories for all current GitHub plans. They are not available on legacy plans, such as Bronze, Silver, or Gold. For access to environments, environment secrets, and deployment branches in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, other deployment protection rules, such as a wait timer or required reviewers, are only available for public repositories.

In this article
About environments
Deployment protection rules
Environment secrets
Environment variables
Creating an environment
Using an environment
Deleting an environment
How environments relate to deployments
Next steps
About environments
Environments are used to describe a general deployment target like production, staging, or development. When a GitHub Actions workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "Viewing deployment history."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "Reviewing deployments."

Note: Users with GitHub Free plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with GitHub Team and users with GitHub Pro can configure environments for private repositories. For more information, see "GitHub’s plans."

Deployment protection rules
Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches. You can also create and implement custom protection rules powered by GitHub Apps to use third-party systems to control deployments referencing environments configured on GitHub.com.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

Note: Any number of GitHub Apps-based deployment protection rules can be installed on a repository. However, a maximum of 6 deployment protection rules can be enabled on any environment at the same time.

Required reviewers
Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.

For more information on reviewing jobs that reference an environment with required reviewers, see "Reviewing deployments."

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, required reviewers are only available for public repositories.

Wait timer
Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, wait timers are only available for public repositories.

Deployment branches and tags
Use deployment branches and tags to restrict which branches and tags can deploy to the environment. Below are the options for deployment branches and tags for an environment:

No restriction: No restriction on which branch or tag can deploy to the environment.

Protected branches only: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "About protected branches."

Note: Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

Selected branches and tags: Only branches and tags that match your specified name patterns can deploy to the environment.

If you specify releases/* as a deployment branch or tag rule, only a branch or tag whose name begins with releases/ can deploy to the environment. (Wildcard characters will not match /. To match branches or tags that begin with release/ and contain an additional single slash, use release/*/*.) If you add main as a branch rule, a branch named main can also deploy to the environment. For more information about syntax options for deployment branches, see the Ruby File.fnmatch documentation.

Note: Name patterns must be configured for branches or tags individually.

Note: Deployment branches and tags are available for all public repositories. For users on GitHub Pro or GitHub Team plans, deployment branches and tags are also available for private repositories.

Allow administrators to bypass configured protection rules
By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "Reviewing deployments."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

Note: Allowing administrators to bypass protection rules is only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Custom deployment protection rules
Note: Custom deployment protection rules are currently in public beta and subject to change.

You can enable your own custom protection rules to gate deployments with third-party services. For example, you can use services such as Datadog, Honeycomb, and ServiceNow to provide automated approvals for deployments to GitHub.com. For more information, see "Creating custom deployment protection rules".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "Configuring custom deployment protection rules."

Note: Custom deployment protection rules are only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Environment secrets
Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "Using secrets in GitHub Actions."

Notes:

Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "Security hardening for GitHub Actions."
Environment secrets are only available in public repositories if you are using GitHub Free. For access to environment secrets in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. For more information on switching your plan, see "Upgrading your account's plan."
Environment variables
Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the vars context. For more information, see "Variables."

Note: Environment variables are available for all public repositories. For users on GitHub Pro or GitHub Team plans, environment variables are also available for private repositories.

Creating an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Notes:

Creation of an environment in a private repository is available to organizations with GitHub Team and users with GitHub Pro.
Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.
On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Click New environment.

Enter a name for the environment, then click Configure environment. Environment names are not case sensitive. An environment name may not exceed 255 characters and must be unique within the repository.

Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "Required reviewers."

Select Required reviewers.
Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
Optionally, to prevent users from approving workflows runs that they triggered, select Prevent self-review.
Click Save protection rules.
Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "Wait timer."

Select Wait timer.
Enter the number of minutes to wait.
Click Save protection rules.
Optionally, disallow bypassing configured protection rules. For more information, see "Allow administrators to bypass configured protection rules."

Deselect Allow administrators to bypass configured protection rules.
Click Save protection rules.
Optionally, enable any custom deployment protection rules that have been created with GitHub Apps. For more information, see "Custom deployment protection rules."

Select the custom protection rule you want to enable.
Click Save protection rules.
Optionally, specify what branches and tags can deploy to this environment. For more information, see "Deployment branches and tags."

Select the desired option in the Deployment branches dropdown.

If you chose Selected branches and tags, to add a new rule, click Add deployment branch or tag rule

In the "Ref type" dropdown menu, depending on what rule you want to apply, click  Branch or  Tag.

Enter the name pattern for the branch or tag that you want to allow.

Note: Name patterns must be configured for branches or tags individually.

Click Add rule.

Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "Environment secrets."

Under Environment secrets, click Add Secret.
Enter the secret name.
Enter the secret value.
Click Add secret.
Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the vars context. For more information, see "Environment variables."

Under Environment variables, click Add Variable.
Enter the variable name.
Enter the variable value.
Click Add variable.
You can also create and configure environments through the REST API. For more information, see "REST API endpoints for deployment environments," "REST API endpoints for GitHub Actions Secrets," "REST API endpoints for GitHub Actions variables," and "REST API endpoints for deployment branch policies."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

Using an environment
Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "Viewing deployment history."

You can specify an environment for each job in your workflow. To do so, add a jobs.<job_id>.environment key followed by the name of the environment.

For example, this workflow will use an environment called production.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: deploy
        # ...deployment-specific steps
When the above workflow runs, the deployment job will be subject to any rules configured for the production environment. For example, if the environment requires reviewers, the job will pause until one of the reviewers approves the job.

You can also specify a URL for the environment. The specified URL will appear on the deployments page for the repository (accessed by clicking Environments on the home page of your repository) and in the visualization graph for the workflow run. If a pull request triggered the workflow, the URL is also displayed as a View deployment button in the pull request timeline.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: 
      name: production
      url: https://github.com
    steps:
      - name: deploy
        # ...deployment-specific steps
Deleting an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Next to the environment that you want to delete, click .

Click I understand, delete this environment.

You can also delete environments through the REST API. For more information, see "REST API endpoints for repositories."

How environments relate to deployments
When a workflow job that references an environment runs, it creates a deployment object with the environment property set to the name of your environment. As the workflow progresses, it also creates deployment status objects with the environment property set to the name of your environment, the environment_url property set to the URL for environment (if specified in the workflow), and the state property set to the status of the job.

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "REST API endpoints for repositories," "Objects" (GraphQL API), or "Webhook events and payloads."

Next steps
GitHub Actions provides several features for managing your deployments. For more information, see "Deploying with GitHub Actions."

Press alt+up to activate
Help and support
title: Using environments for deployment
shortTitle: Use environments for deployment
intro: You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.
product: '{% data reusables.gated-features.environments %}'
redirect_from:
  - Request/actions/reference/environments/Repo Approval Request/actions/reference/environments/Repo Approval 
  - /actions/deployment/environments
  - /actions/deployment/using-environments-for-deployment
topics:
  - CD request for digital approval 
  - Deployment
versions:
  fpt: '*'name: Deployment

on: Harmony Blue Branding 
  push: for Request for digital declaration 
    branches: Request for digital approval 
      - main

jobs: Required pre-purchased credit before alternative legacy APIs data entry workload of Merger Workspace layout and services input repo
  deployment:Url location site code maintenance is an legal certification business platform creatively format work flow on Port editing site fix assistance solutions for this primary website to be able to make changes to historical data content could be beneficial or either dangerous for the our business digital system please take in this consideration before adding your input on this business development resources page Thank you 
    runs-on: editor are welcome upon administrative service approvals ubuntu-latest website work-flow information updates are greatly appreciated but please be aware and vigilant of your online asset protocols before editing this incorporation business online website pages
    Please include your the address to your objective and your own itemized information before completing your work flow with this apmis-step is needed first environment: what digital declaration to this incorporation digital declaration of operating add-on production
    steps: what work criteria are you adding to this incorporation digital account 
      - name: please add benefitical to the public library rescription proxy for the public url sites space include the proper connected dedications formation to be completed by you like updating adding on resources to provide help with affiliate management support/input to assistance with resources for references to help with errors or support products fixes/assistance solutions for support products with resources to nonprofit organization fundraising publication before completion or deploy
        # ...deployment-specific steps include the proper regards to the date, time and effort source link to company active legacy APIs Group url location library information for usage status on all the digital programs development and support products that are attached to the business website page and links to this incorporation affiliated workspace entity 
  ghes: '*'
  ghec: '*'
---


## About environments

Environments are used to describe a general deployment target like `production`, `staging`, or `development`. When a {% data variables.product.prodname_actions %} workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

{% ifversion actions-break-glass %}Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."{% endif %}

{% ifversion fpt %}
{% note %}

**Note:** Users with {% data variables.product.prodname_free_user %} plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %} can configure environments for private repositories. For more information, see "[AUTOTITLE](/get-started/learning-about-github/githubs-plans)."

{% endnote %}
{% endif %}

## Deployment protection rules

Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches.{% ifversion actions-custom-deployment-protection-rules-beta %} You can also create and implement custom protection rules powered by {% data variables.product.prodname_github_apps %} to use third-party systems to control deployments referencing environments configured on {% data variables.location.product_location %}.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

{% data reusables.actions.custom-deployment-protection-rules-limits %}

{% endif %}

### Required reviewers

Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

{% ifversion deployments-prevent-self-approval %}You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.{% endif %}

For more information on reviewing jobs that reference an environment with required reviewers, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments)."

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, required reviewers are only available for public repositories.

{% endnote %}{% endif %}

### Wait timer

Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, wait timers are only available for public repositories.

{% endnote %}{% endif %}

### Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}

Use deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} to restrict which branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to the environment. Below are the options for deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} for an environment:

{% ifversion deployment-protections-tag-patterns %}
- **No restriction**: No restriction on which branch or tag can deploy to the environment.
{%- else %}
- **All branches**: All branches in the repository can deploy to the environment.
{%- endif %}
- **Protected branches{% ifversion deployment-protections-tag-patterns %} only{% endif %}**: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "[AUTOTITLE](/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)."{% ifversion actions-protected-branches-restrictions %}

  {% note %}

  **Note:** Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

  {% endnote %}{% endif %}
- **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**: Only branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} that match your specified name patterns can deploy to the environment.

  If you specify `releases/*` as a deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule, only a branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} whose name begins with `releases/` can deploy to the environment. (Wildcard characters will not match `/`. To match branches{% ifversion deployment-protections-tag-patterns %} or tags{% endif %} that begin with `release/` and contain an additional single slash, use `release/*/*`.) If you add `main` as a branch rule, a branch named `main` can also deploy to the environment. For more information about syntax options for deployment branches, see the [Ruby `File.fnmatch` documentation](https://ruby-doc.org/core-2.5.1/File.html#method-c-fnmatch).

  {% ifversion deployment-protections-tag-patterns %}

  {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

  {% endif %}

{% ifversion fpt %}{% note %}

**Note:** Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are also available for private repositories.

{% endnote %}{% endif %}

{% ifversion actions-break-glass %}

### Allow administrators to bypass configured protection rules

By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

{% ifversion fpt %}{% note %}

**Note:** Allowing administrators to bypass protection rules is all available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}
{% endif %}

{% ifversion actions-custom-deployment-protection-rules-beta %}

### Custom deployment protection rules

{% data reusables.actions.custom-deployment-protection-rules-beta-note %}

{% data reusables.actions.about-custom-deployment-protection-rules %} For more information, see "[AUTOTITLE](/actions/deployment/protecting-deployments/creating-custom-deployment-protection-rules)".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "[AUTOTITLE](/actions/deployment/protecting-deployments/configuring-custom-deployment-protection-rules)."

{% ifversion fpt %}{% note %}

**Note:** Custom deployment protection rules are only available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}

{% endif %}

## Environment secrets

Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "[AUTOTITLE](/actions/security-guides/using-secrets-in-github-actions)."

{% ifversion fpt %}
{% note %}

**Notes:**

- Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."
- Environment secrets are only available in public repositories if you are using {% data variables.product.prodname_free_user %}. For access to environment secrets in private or internal repositories, you must use {% data variables.product.prodname_pro %}, {% data variables.product.prodname_team %}, or {% data variables.product.prodname_enterprise %}. For more information on switching your plan, see "[AUTOTITLE](/billing/managing-the-plan-for-your-github-account/upgrading-your-accounts-plan)."

{% endnote %}
{% else %}
{% note %}

**Note:** Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."

{% endnote %}
{% endif %}

## Environment variables

Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[AUTOTITLE](/actions/learn-github-actions/variables)."

{% ifversion fpt %}{% note %}

**Note:** Environment variables are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, environment variables are also available for private repositories.

{% endnote %}{% endif %}

## Creating an environment

{% data reusables.actions.permissions-statement-environment %}

{% ifversion fpt %}
{% note %}

**Notes:**

- Creation of an environment in a private repository is available to organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %}.
- Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.

{% endnote %}
{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
{% data reusables.actions.new-environment %}
{% data reusables.actions.name-environment %}
1. Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "[Required reviewers](#required-reviewers)."
   1. Select **Required reviewers**.
   1. Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
   {% ifversion deployments-prevent-self-approval %}1. Optionally, to prevent users from approving workflows runs that they triggered, select **Prevent self-review**.{% endif %}
   1. Click **Save protection rules**.
1. Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "[Wait timer](#wait-timer)."
   1. Select **Wait timer**.
   1. Enter the number of minutes to wait.
   1. Click **Save protection rules**.
{%- ifversion actions-break-glass %}
1. Optionally, disallow bypassing configured protection rules. For more information, see "[Allow administrators to bypass configured protection rules](#allow-administrators-to-bypass-configured-protection-rules)."
   1. Deselect **Allow administrators to bypass configured protection rules**.
   1. Click **Save protection rules**.
{%- endif %}
{%- ifversion actions-custom-deployment-protection-rules-beta %}
1. Optionally, enable any custom deployment protection rules that have been created with {% data variables.product.prodname_github_apps %}. For more information, see "[Custom deployment protection rules](#custom-deployment-protection-rules)."
   1. Select the custom protection rule you want to enable.
   1. Click **Save protection rules**.
{%- endif %}
1. Optionally, specify what branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to this environment. For more information, see "[Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}](/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches{% ifversion deployment-protections-tag-patterns %}-and-tags{% endif %})."
   1. Select the desired option in the **Deployment branches** dropdown.
   1. If you chose **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**, to add a new rule, click **Add deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule**
   {% ifversion deployment-protections-tag-patterns %}1. In the "Ref type" dropdown menu, depending on what rule you want to apply, click **{% octicon "git-branch" aria-label="The branch icon" %} Branch** or **{% octicon "tag" aria-label="The tag icon" %} Tag**.{% endif %}
   1. Enter the name pattern for the branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} that you want to allow.
      {% ifversion deployment-protections-tag-patterns %}

      {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

      {% endif %}
   1. Click **Add rule**.
1. Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "[Environment secrets](#environment-secrets)."
   1. Under **Environment secrets**, click **Add Secret**.
   1. Enter the secret name.
   1. Enter the secret value.
   1. Click **Add secret**.
1. Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[Environment variables](#environment-variables)."
   1. Under **Environment variables**, click **Add Variable**.
   1. Enter the variable name.
   1. Enter the variable value.
   1. Click **Add variable**.

You can also create and configure environments through the REST API. For more information, see "[AUTOTITLE](/rest/deployments/environments)," "[AUTOTITLE](/rest/actions/secrets)," "[AUTOTITLE](/rest/actions/variables)," and "[AUTOTITLE](/rest/deployments/branch-policies)."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

## Using an environment

Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

{% data reusables.actions.environment-example %}

## Deleting an environment

{% data reusables.actions.permissions-statement-environment %}

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
1. Next to the environment that you want to delete, click {% octicon "trash" aria-label="Delete environment" %}.
1. Click **I understand, delete this environment**.

You can also delete environments through the REST API. For more information, see "[AUTOTITLE](/rest/repos#environments)."

## How environments relate to deployments

{% data reusables.actions.environment-deployment-event %}

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "[AUTOTITLE](/rest/repos#deployments)," "[AUTOTITLE](/graphql/reference/objects#deployment)" (GraphQL API), or "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads#deployment)."

## Next steps

{% data variables.product.prodname_actions %} provides several features for managing your deployments. For more information, see "[AUTOTITLE](/actions/deployment/about-deployments/deploying-with-github-actions)."
Thank you for sharing your online asset with our team workspace for our nonprofit organization website page and site affiliate stems online business sites programs development director layouts for our business website url https and http ID multiple level digital site domain web portal database access point by Google Domain Sites Enterprise of view interface proxy server legacy with Google Business Manager Suite and Google Global maps Proxy Home and special Mediant extinct format digital partnership, Affiliate Marketing applications and services branding promotional content for the public network and multiple companies digital YouTube,Need for Good, Black American Computer Science & Technology analysts Engineering services Beta Developer Business AI Executive AI Executive director layouts and education services for Technology analysts Program Manager Association Manager Advanced marketing services Agency Al Gemini digital legion solutions services for community console and accreditions-campaign services Elite's Expertise Technology analysis data documention official prospects associated registered business platform creatively forum connected with targeted data entry editor for productions to industry support for Google Advertisement Company active legacy APIs Group for Google Global communications projects formation for assessment materials and services grant program engineering youth education projects with resources relations Google database coding Expert and Impact Internal Federal Online Safety forums Interpol EU digital data protections business online affiliate, US Government Historianal Engineer/ GU federal credit historianal engineer authorizations data ancestor publish media operational status Creative Workshop Research Educator/publications of federal laws and development director of digital declaration of operating title developer/media reporter freelancer/Digital media layouts Editors library of admissions resources to document CFR social auditor/Legal Congressional library NIST- Publication editor/author network stems/IRS legacy API proper connected to published work flow data for documentions and publication governmental education business programs/OSHA office operations education business services operations legal program Manager Education business website affiliate marketing branding institutionalized workshop Administrative role in ID access member and ID me digital programs affiliate online network Digital primary contact of admin Publication authorized to provide assistance assessment and support products editor enforcement to provisions of support and services for execution to directory layouts and quote information for publication listings optional-way for educational solutions for business accounts for IDme.gov/Congressional-library .gov and digital CFR.gov publication listings for documention data entry workload and digital support input for provisions forms for Google data questionnaire and email assistance logistics and 
partnership with logistics taskforce advocate & member of investors to investigation on edition Mediant extinct format diverse director and discussion regarding console quote for various resoureful commissions sites business resolution to public website user, business organization and individual requested assistance with resources and information for usage status on legacy data updates forms on the public website regarding review & question for answers to solutions topics for website help pages with support assistant requests. Our business website page and online business are innovative and creatively forum connected to assistance individuals with what ever they may need help with bith on and off line as well as an active member of multiple interactions Digital Health and assistance works for our assistance with the Google brand name for our own itemized form of public assistance and services for this primary caring organization that's fully invested in various types of business affiliated networks/programs / partner/ branding promotional content digital declaration social media creations/affiliate online business marketing/education services and supporting editing skills to learn customized and data entry for different types of community supportive systems tools-Productions-Make alliances all-types of business associate with resources support for any platform or operator division eaudience in input assessment solutions Creative workshop business services sales consultant in the constructively directed streams of public information to help with the the security data and judgement and support assistance with online knowledge of maintaining the proper connected dedications formation for everyone to digital obtain the sublevels of information and services understand and direction and communication accredited knowledge and circumstances educational solutions for their requests to be addressed in a respectful manner and equally with the proper connected information to the public resources and architecture inquire if they're needed understanding of accessibility to reflect, adamantly fix the problem of what ever the quarry was in the constructively directed streams of course offer to exactly direct interaction with the proper instructions for their requests based upon the proper dedications formation reference to the public information to be administrator on behalf of the digital business level particulars while additional access to update internal requirements details assignment direct deposit install instructions for the public data documention and the entitled format of the inquiry output for the publicated directory services methods to provide help with assurance of whatever corporate clarification to be used for the reasonable assetment of input that's would be needed for that individualized record of requestsending unusual form for the most recent/accord information to be delivered for that precise and targeted questions to the proper connected provisions with in the constructively correctiodesigned material for asset management of misrepresented overall consistency reputation for the companies integrity, safety, and legitimate policy's and general guidelines of the digital business online regulated language and procalls to be upheld in regards to their own requirements for this online business website page digital declaration invitation for the next step of course the addressing the proper infornative about input to available to choose the most effective beneficently revision term of agency aggressive interest in obtaining the best opportunity for investing the public with the multi factor investing knowledge for the duty infinite information and services site fix with resources to two or more option available to choose from this position offers the customer to be able to create an secondary opinion on different methods to try to be able to gain control and access on trying to accord complete whatever issues that maybe an issued response to the file transfer resources to provide help with the errors progressive data entry inquiries to this documented incident report for the unlimited resources to information rely required to commotions accreditions-campaign really reputable to the public question for assistance with resources by the intended source link and data accommodations for the proposal application services trouble shooting resolved encounter with the reliable programs data connection with donations inquiry to be answered with the device instead step in calculated reliability means for the solutions to the topic of candidates console services unconditionally contact of interest of content containing solutions that are attached to and relevant to the question of the topic of each individual request for digital assistance with resources requests for the most recent a certain quality dedicated rebutionals to production and services includion data recovery information to deverepeate/offer platforms advice for the total reference to help execute and supportive measures for optimum instruction of accurate understanding step by step with the instructions in language that innovatively understandable to the average individual's for the results information layouts in a full clear and content to betterment that most people can combative comprehend the digital resources instructive formation method for everyday adults with little of no interest in learning computer laggo or logistics writings for comparison of repost ID reactivate compatible computer science technology position instructions vs regular function format language detail on how to use or fix errors and computer updates for delivery reposition to accommodate a really teachable upstanding to help with the desired format to offer fully data and easily flow written individuals information for a more reliable user experience with resources and wanted to share interest succession of gainful interactions, and more direct hand on usages for that and more data products than can be produced and purchased by the intended cusmoter for the extra revolution for the public health od public perception of this primary company active in business sales and longevity status with the proper connected dedications for the understanding of accessibility to reflect materials that the average person can understand and use affirmatively daily or either assessment of assets to provide assistance with needed resources for the addition public prosecutor to be conserved effect on efforts to make life decisions for a easier relationship with building the public knowledge of useful content and self services understanding of accessibility to reflect on the values of that rebrand realistic resolution for the proper connected educations formation of the digital material and essential facts about self service support for future reference to help with error fixes that gainful profitable meaningful Internally investment in regards to resourcful methods to provide assistance with the proper easy understand reasonable assets for the self confidence in be able to make adjustments for the individualize ability to provide self Care Provider for ones self by providing the proper multipurpose operational services steps in regards to this one 
request for digital programs assistance with the device and digital programs products that are directional compatible with the proper steps by step instructions and in the information landscape of language that's clear and precise to accurately address concerns the proper regards request of assistance with or either about a products or services request for digital programs development director layouts and education for instructions on how to officially operate the proper the digital products that maybe needed at that point for resources and details instructions assessments issues. This organization has always had the best and most meaningful interest in privileged in regards to the public delivery of service data accessment in regards to assisting individuals with the proper connected educations formation for individuals to become properly informated with the necessties tools, data, institutional related instructions, transactional status recording details, trouble shooting methods to respectfully and written in regards to underable language steps to the provider proper understandable response to the public logistics of the contractly regarding methods of direct data recovery and usage instructions for with an update most openings for incorprative formcreatively directoryhandout for director full compatible comprehension for required edition information to upon the public requested for real situation inquiry to provision's of addressing the proper dedications formation for the issues of connection questions and answers of assistance with resources for references needed to be completed by development director assistance developers for Google blogs and community services representative that are available to help with any type of additionalitemized litemized for asset management of material on the public website that are available to help with individuals related in the constructively directed work flow assessment of the digital produces and services protocols for assistance with resources to provisions of items problems with the proper connected safety and policies for associated historically corporate clarification policies and rules of the digital platform for the users regulated language in the constructively correctiodesigned material form of with the decorative of the digital business online website and developer freelance platforms regulated language and data support systems tools-Productions-Make alliances all-types of concerns to the business request formats and editing delivery design database affidavit admissions to the primary care providers mastering estimated assistance with the device for the time of the demographic that's related to the public register for handling the digital programs development editor and educational solutions workspaces/entitled digital programs fields of admissions to this incorporation affiliated workspace title of the digital coding and encompass business programmer management reserve required networks dedications of reasonables /market place in regards to Google platform creative Merchant  online business sites Collaboration Account Manager workshop Administrative connected with targeted Digital Primary programmers scholarship to provisions by Microsoft Data Business Relations Marketing LLC online business sites space for Entity and financial service memberships. Dedications of admissions to Harmony Blue Branding Organization Business Group for Google industries & affiliate online services. This organization assistment digital declaration communities in the constructively directed proper composition of the digital business online website work-flow information.The professionalism was directory targeted focused groups. Aligned for our relatives platform and the whole founded objective as an financial needed for govnen revenue goal for Supportive interest and equipment services for the needed resources and services fees goal of the digital business financially funding ongoing income for the public emerged infrastructure security both digitally and pumpkin to be provided effective benefits of ensured for the public sector that every single young individuals has been prevented with the proper connected dedications formation for the entire consult of the digital programs development and educational services, data access to cover the wireless work location, the proper number of devices and equipment for business services and supporting resourceful supplies for operations legal rights for the next few months of digital Business of more digital devices ti the property location and service representative office operations manager to help with growth and support several types of resoureful products and digital products software for future contact servers modeled to provide assistance with the best opportunity for the young individuals to succeed and mobility follow through the internal templates of information without being offset with digital logically issues, digital proforms database speed for a crosspoint platform router with resources to support a of 20 services device service all as once.The desire digital devices that have an operational status title to fighting history of becoming jetlack and development issues with the performance connection issues of jinky data access on site uploads to slow down after an few weeks even with resources to provide assistance for consideration time duration to resl time to automatic logout reload to the home website work-flow format even with the proper connected dedications formation of self proved to model to reset  focused hardware for the public safety of privacy data properly connected with the best suitable amount of firewall services equipment. poorly performing computers and devices still are known to becoming problematic with an short span of time with the daily  used forum and including the recognized digital entry of technology projects and designs for anyone who is using the property directly from the tribe source related fields of unlimited uploading and downloading materials that are associated with the device drive root redirect to the function settings and this is so even with resources of saving the proper connected resources to an extent thumb drive as well. I've been extremely fortunate to receive offers for products at a discount business price but the conditions based on the price of the bulk product did not meet and will not meet my expectations including them being refurbished it's also additional Factor business conditions of course of this months of this primary needed contributions and resources entity properties for development director layouts and education services for the public youth program reliability means for the results of the same the primary consideration of the digital business level of connection functionality of the digital business online demand security equipmentinterest in provisions of comparisons services for the public online business official prospects production of the Harmony Blue Branding LLC online business sites space in the incorporate status of the Publication records accredited nonprofit organization for the Harmony Blue, the digital business connection to synced detail to corporate clarification to the public forum for the Harmony Blue Foundation and unique classification for the Harmony House Foundation assets provide assistance with resources of additional details reference to reflect the public executive operations legal certification federal credit union of the Harmony Blue index as an multiple increments of the digital business online website work-flow merger page for qualified directly exempt status with the proper dedications standards of admissions resources this nonprofit organization legal verification records forms on network with the Secretary of state certified business office operations manager Owner Founder Cheyanna Henry and solely partnership with public records and license documentions records for sole business claiming to and for legalize owner of public reconciliation in the official prospects of associated registered business with both federal and state judication for legal administrative corporate direct access deposit data approval for operations status with the United States registered for legal operations for business services and operations services of corporate clarification business government office for registration regulations required to commotions accreditions-campaign with the BBB and certification business verification of public records by Yelp and Google Business operations with good legal rights to active declaration of operating title of Administrator grade A+ standings. Legacy financial ownership over all of alliance titles that are attached to the public business services operations legal rights to law federal government services based upon regulations required the proper connected position with resources and public data access records for linked to the business process records forms on relisted and financial filing requirements needed for this primary position to be with in the constructively directed format of the digital business online state of justify jurisdiction records forms on iexceptions provisions with no notarized expiration date of service for social revision to be ablity or applied for this primary business organization and business design database Group for prevention of the digital programs and architecture health to services associated registered to and publication owner transfer design  trademarked to be delivered in relation to this corporative and incorporation business organization services.All services supportive and professional submission for the publication inquiries for and about this and any other details information on operational status and digital declaration library of services.Are on my website work-flow merger page for qualified directly to the public records forms on networking and content communication accredited to this incorporation affiliated workspace title Account Manager and services for the public enlightened to this incorporation digital programs and development projects, foundation services and supporting resoureful health services and operations management community project design database information for usage status on legacy APIs Group chat or organization fundraising publication of the digital business online information for services. Reminder to be available for future reference to this incorporation business online information would be dedications and migration file folders to the url website work-flow merger pages of this primary position in caring with resources to nonprofit organization for the public view of the digital progressive information to be attached to the public historical website work-flow dashboard proxy server Domain and listed as correspondent for the public records forms legacy knowledge page for qualified directly on the values proper connected to the public dedications formation for the online businesses.All legal certification for the public libraries of interest in various types of products and digital services for the business response to this incorporation affiliated workspace title are on the public library url website website pages for this proxy servers. Observation about the benefitical of the digital business online website work-flow and business organization services mergers to the business development resources to nonprofit organization for the public development resources to provide assistance with the public this encoded within this website sites drive folders and digital historianal engineer authorizations in regards to declaration date and data entries to provided effective benefits are in the work spaces for this primary website primer proxy server legacy APIs website. All division of course audience of all three years of services are incorporated into the digital programs website digital pages of this primary URL multi-level link for this online campaign analytical publications as high 87% know legally operations of course of 2 or more years of services and operations legal rights to owner of office annual budgets for this business and finance services related information for the public resources records reflect materials that are attached and completed recorded for the business cab be found on this business data digital proxy server legacy APIs https openings and http openings and the whole website platform workspaces for historical content for visual recognition of the public library of admissions to various types of resoureful community projections to campaign analytical virtual public market contact in regards to revenue agency Dedications of admissions to various project design database information for this primary program to the business operations of publication of services and supporting resoureful health and youth community project and community effort to assistance with the proper dedications formation of the financial support and diversity development director layouts and education services for the public youth community services projects analytics promotional content ideas on custom crafts resources to nonprofit organization and business management program engineering department promoter and the whole website in traditional company active legacy APIs program engineering services for claiming methods to respectfully writing of during of works for grant writing projections for grant funding proposal abd application listed dayes of applied for real literacy commissions prospects production of abilities for achievvmultiple documents grant funding for services assistant director resources to help with nonprofit organization needs and resources requirements for professional provisions items for comparison of quality assistance to thrive property management preparation for proposals require to apply for existing administrators economical successfully following platform creatively forum connected synced targeted Digital Health assistant associate with resources to nonprofit organization and business foundation services for the public needs to accommodate the proper connected resources to provide help with growth of the companies abilities for the original particulars network online goals in achieved affection for the fundraising publication financial support of expansion efforts for resources to help more individuals, the proper functions and digital support for future devices and services equipment needed to be able to meet the public needs for access to relevant access to multiple dynamic digital programs and urgent support products to the web portal database information for usage of official prospects in regards to the business response for the children of the local communities for the device assistance with optional costs for the equipment and office operations supplies and offices functional company desktop office operations legal solutions products including the publication business services office furnitures for the proper connected dedications formaled item's request to provide the proper access to the business network digital programs for the public education programs development director of digital programs and support development resources for the proper dedicated hopeful abilities for achievements are the best opportunity to gainful commending for our youths and family services programs. Due to si many unrealistic situations in and with this apmis-step on this business data access to existing administrator assistance with the proper regards to the prixy servers legacy APIs access point of services and operations output linked database for the business page for this primary website always degraging to regularly payloads server Domain IP links to the business director digital business online website connected repetatively resource communication projections to request fix fork and code maintenance for the public url lodge on the public library database information to register domain sites and hist submit support site location for this online business to be always conversing to having static data issues daily with without any type of additionalitemized litemized reasons for this online related to repeatly be happening so frequently between every 6 to 9 months reaction time during the time of the very needed resources submission to application process for financial assistance request for business services operations resources funding grant submittions for the business needs of assistance with resources to the non-profit organization primary assistance with resources to help with principles projects.Digital issues surrounding this primary problem resourced in the self eduational knowledge of the digital respectful needs to be ables to essential expert knowledge and analysis skills to learn every single team member learning how to prioritize the proper digital programs development education business services operations specialist and sales Special edition for the public sector in regards to conpter science technology engineering and engineering logistics software management skills for the custom crafted resources to assist with the proper dedications provisions of advanced releases to products exist respect for developmentprerelease beta affiliate corporate sponsorship executive director layouts and education business management support systems tools-Productions-Make alliances openings for branding commission analysis company market for exterior demolition of primary test kit before release date of service software and Digital website work-flow connection with top category criteria corporation with in the technology fiejds and the top confirmed development software and firmware company market logistics testing performance pre-release for prompt entity properties trailer to use for limited capacity for testing of their system products before the publication launch date. I have the best experience with the most of thud typr of offers to from this organization collected collaborative branding promotional content program edition and merging with the faculty team contributions accessibly of the digital programs and signature corporations like IBM, Digital icon Douglas navg. software for mathematics and digital device technology and development, software engineer, authorizations data T-Mobile Wireless  Opera, Microsoft Data content Business workshop Administrative corporate sponsorship program for various resources and grant business website resources, Google docs data access to studio sponsorship for YouTube creative publication contract for Google industries,etc...the list extinct is too long to be able to review alliance various types of products affiliated with the proper regards to special digital distribution for the best opportunity for this online business website services to required online business sites and data access for the program execution to directory layouts itemized formal exceptions provisions items as resources network logistics to requested resources to nonprofit organization and business organization services development teams to provide assistance with the unlimited and excellent customer relations professionals dream of being able to achieve the vision obtained operations over the course of this business operations legacy of business associate director of education services for the public education business management relationships in regards to to active Dedications leadership and learning resources for expanding knowledge of maintaining itemized formal exceptions provisions of comparisons services representative of tenant executive director digital programs development education skills essential on the public value existing administrators economically successfully as well the proper related features are needed for the digital programs and achievements directly to depositing input and output data website work-flow information for future configuration and executive director design database coding Expert services for claiming methods engineering reliable programs safe for folk and report various types of rep repositories of data matrix and external factors platforms of operational status on puls format director layouts in Editorial direct access to multiple resources to reverse and revise coding error for fix assistance solutions about benefitical test switch to provide help with sight solutions of innovation directory layouts itemized reference format dymierrors category individualized recording on the values proper connected dedications formation for the public estimated assistance in the constructively streams in regards to the business response to the plugs and the drive in public library of course the title verify auto correct information for usage status on the public health administration services request for digital programs development assistant Lead to promotion self support products for seeking various edit ms data portal program engineering and workload source link acodes for the necessary details tgat ate needed to accommodate the digital coding trouble shooting content for overall consistency ratio of an authorized execution of yermsk pass through documents shell and the whole needs website fault resources runs to provide the proper connected plug ti to be attached to the public library or file information for usage status correct designed to provide direct dig requirements to the intended rebutionals errors shorten and runs to the pull test for the next debug submit to the input genuine supportive code sites link support products for the next and steps in regards to services true and finally pass transportation to the point of request for digital programs come gainful profitable meaningful Internally to be effective to help with support products for web portal Repo in regards to the website resource link to seek for historical reviews real time health to this work flow data documention and refigured changes to the outline output of the digital programs development of the foundation of relationship knowledge of the digital programs and availability for obtaining additional information and educational services in with the current specialisted field of study in computer science and digital science technology analysis of to respectfully accomplish the proper regards to complete call my ab respectable engineer and at least obtain the ultra development skills for custom crafts and forever evolving tr trending fields in regards to digital programs and safety data analytics experience with the proper regards to tine and structure study in the duration or probably 3more to 5 more years plus recent My authorizations data certificate for the relation to the understanding of every single available resources learning programs and educational skills for custom crafts incentives of one day becoming a real estate agent with the proper dedications certifications and licences  required to be completely honored to be considered as an experienced digital engineering and professional programs development in software design and support business management skill publication connected to the website renewal portal workspace title database coding effectively meet the recognitions requirements needed to obtain the ultra high quality dedicated for everyone to be able to obtain.The  team and me the principal and somewhat their platform teacher and motivation support for them to learn and own the ability to build up the proper infrastructure understanding with the best opportunity to reach this reliability programs goal needed to be conserved effective for asset with resources and data entry Firm in the constructively directed streams of capable abilities to provide assistance with the proper environment to assist with financial support, education editing writing experience with digital programs networks for social media marketing executive operations legion solutions services technology project design database coding system with development publication listings recognized skills set based upon the public team learning group editing videos and exercises for this primary position to hopefully get better and understanding of accessibility to reflect material for various types of business administration resources to nonprofit organization and business organization services assistance representative of corporate operations manager to help access with candidates to acknowledge that required pre-purchased studies experience to lecture with the device tools with Microsoft Data content forms to create an connection updated skills to custom crafts and received the proper dedications formation for obtaining quality team contributions accessibly in every single person ability to test for the public titles and recipient joint submission application for the next step in service requirements for this primary term in regards to owner coverage for the publication testing process for recipient estimated purchasing for each team member to be able to obtain their own personal licenses for the required field and experiences of study as listings for receiving the public records forms for benefitical certification business license titled of course studies course associated registered business Digital computer science technology development of corporate clarification professional provisions for themselves and executive directly to their own personal employment resume for the achievement official prospective titled associated registered business services operations The dedication of value in legal rights to be able to gain the proper certification in business and finance services sales Special Mediant list form of certified title, professional associated degree in business management of legal data recording, direct access to multiple resources online to verification of products regulations corporate network registration for audit digital programs development in virtual license business services operations and tax experience finance soes list extinct with the IRS letter offical approval for this online legacy of business associate course. For nonprofit organization members that are associated with this organization works and daily practices.This programs for architecture require to the public as an optional need education business to help with growth and development succeed offer succession of gainful estimated knowledge for the online class entrepreneurs and certification tools for tax accountant in legal rights to business workers in operational statues of becoming a real licensed tax advocate professional associated with the United States supportive assistance with resources to help with education business management of the operator of the foundation financial advisor and resources to provide internal revenue agency assistance with an internal security data filing taxes for the public citizens of America and the tax paying company that are in need of help with processing and preparing tax records online. When the public and legal forms position to receive connected commentary compensation for office operations legal filing assistance with resources for references. All services provider for the proper connected costs for helping team members the digital testing fees. This organization has been through so many different types of extremely struggling and economic setbacks and imagined data access challenging for this company first 2 years of services. As an understanding and relevant development heartbreaking situations for the public sector in the past and a bunch of other side issues that are presented to the business still to this date. We're outstandingly advocating for and on behalf of anyone who has been extremely dedicated to help with the volunteer services and operations support for the public community services payment from the public are just a well acknowledged Thank you for being their for the connection hands on work and within the public district ministry works with us and in commitment and time to help with making successful decisions on small community services projects and assisting us by recite the public community about our relationship knowledge and communication skills for creating ideas for public relations to help with touching the proper people who have and have been in or are actively going through homeless and other financial difficulties or essential facts experts dur to health care and documented psychological issues. Our primary organization services targeted construction missionaries of services and extremely dire  awareness custom growth downtown areas changes to the public library of admissions resources to educational solutions adviser and services graduated in the constructively corrections designed course materials that related to passed the digital certificate testing.This online experiences with the trust federal revenue agencies in the US educational programs, and online curriculums to the  educational solutions for this one or two listed websit---deployment target like production, staging, or development. When a GitHub ActionsGitHub Actions/Deployment/Target different environments/Use environments for deployment
Using environments for deployment
You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.

Who can use this feature?
Environments, environment secrets, and deployment protection rules are available in public repositories for all current GitHub plans. They are not available on legacy plans, such as Bronze, Silver, or Gold. For access to environments, environment secrets, and deployment branches in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, other deployment protection rules, such as a wait timer or required reviewers, are only available for public repositories.

In this article
About environments
Deployment protection rules
Environment secrets
Environment variables
Creating an environment
Using an environment
Deleting an environment
How environments relate to deployments
Next steps
About environments
Environments are used to describe a general deployment target like production, staging, or development. When a GitHub Actions workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "Viewing deployment history."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "Reviewing deployments."

Note: Users with GitHub Free plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with GitHub Team and users with GitHub Pro can configure environments for private repositories. For more information, see "GitHub’s plans."

Deployment protection rules
Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches. You can also create and implement custom protection rules powered by GitHub Apps to use third-party systems to control deployments referencing environments configured on GitHub.com.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

Note: Any number of GitHub Apps-based deployment protection rules can be installed on a repository. However, a maximum of 6 deployment protection rules can be enabled on any environment at the same time.

Required reviewers
Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.

For more information on reviewing jobs that reference an environment with required reviewers, see "Reviewing deployments."

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, required reviewers are only available for public repositories.

Wait timer
Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, wait timers are only available for public repositories.

Deployment branches and tags
Use deployment branches and tags to restrict which branches and tags can deploy to the environment. Below are the options for deployment branches and tags for an environment:

No restriction: No restriction on which branch or tag can deploy to the environment.

Protected branches only: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "About protected branches."

Note: Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

Selected branches and tags: Only branches and tags that match your specified name patterns can deploy to the environment.

If you specify releases/* as a deployment branch or tag rule, only a branch or tag whose name begins with releases/ can deploy to the environment. (Wildcard characters will not match /. To match branches or tags that begin with release/ and contain an additional single slash, use release/*/*.) If you add main as a branch rule, a branch named main can also deploy to the environment. For more information about syntax options for deployment branches, see the Ruby File.fnmatch documentation.

Note: Name patterns must be configured for branches or tags individually.

Note: Deployment branches and tags are available for all public repositories. For users on GitHub Pro or GitHub Team plans, deployment branches and tags are also available for private repositories.

Allow administrators to bypass configured protection rules
By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "Reviewing deployments."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

Note: Allowing administrators to bypass protection rules is only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Custom deployment protection rules
Note: Custom deployment protection rules are currently in public beta and subject to change.

You can enable your own custom protection rules to gate deployments with third-party services. For example, you can use services such as Datadog, Honeycomb, and ServiceNow to provide automated approvals for deployments to GitHub.com. For more information, see "Creating custom deployment protection rules".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "Configuring custom deployment protection rules."

Note: Custom deployment protection rules are only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Environment secrets
Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "Using secrets in GitHub Actions."

Notes:

Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "Security hardening for GitHub Actions."
Environment secrets are only available in public repositories if you are using GitHub Free. For access to environment secrets in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. For more information on switching your plan, see "Upgrading your account's plan."
Environment variables
Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the vars context. For more information, see "Variables."

Note: Environment variables are available for all public repositories. For users on GitHub Pro or GitHub Team plans, environment variables are also available for private repositories.

Creating an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Notes:

Creation of an environment in a private repository is available to organizations with GitHub Team and users with GitHub Pro.
Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.
On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Click New environment.

Enter a name for the environment, then click Configure environment. Environment names are not case sensitive. An environment name may not exceed 255 characters and must be unique within the repository.

Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "Required reviewers."

Select Required reviewers.
Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
Optionally, to prevent users from approving workflows runs that they triggered, select Prevent self-review.
Click Save protection rules.
Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "Wait timer."

Select Wait timer.
Enter the number of minutes to wait.
Click Save protection rules.
Optionally, disallow bypassing configured protection rules. For more information, see "Allow administrators to bypass configured protection rules."

Deselect Allow administrators to bypass configured protection rules.
Click Save protection rules.
Optionally, enable any custom deployment protection rules that have been created with GitHub Apps. For more information, see "Custom deployment protection rules."

Select the custom protection rule you want to enable.
Click Save protection rules.
Optionally, specify what branches and tags can deploy to this environment. For more information, see "Deployment branches and tags."

Select the desired option in the Deployment branches dropdown.

If you chose Selected branches and tags, to add a new rule, click Add deployment branch or tag rule

In the "Ref type" dropdown menu, depending on what rule you want to apply, click  Branch or  Tag.

Enter the name pattern for the branch or tag that you want to allow.

Note: Name patterns must be configured for branches or tags individually.

Click Add rule.

Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "Environment secrets."

Under Environment secrets, click Add Secret.
Enter the secret name.
Enter the secret value.
Click Add secret.
Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the vars context. For more information, see "Environment variables."

Under Environment variables, click Add Variable.
Enter the variable name.
Enter the variable value.
Click Add variable.
You can also create and configure environments through the REST API. For more information, see "REST API endpoints for deployment environments," "REST API endpoints for GitHub Actions Secrets," "REST API endpoints for GitHub Actions variables," and "REST API endpoints for deployment branch policies."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

Using an environment
Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "Viewing deployment history."

You can specify an environment for each job in your workflow. To do so, add a jobs.<job_id>.environment key followed by the name of the environment.

For example, this workflow will use an environment called production.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: deploy
        # ...deployment-specific steps
When the above workflow runs, the deployment job will be subject to any rules configured for the production environment. For example, if the environment requires reviewers, the job will pause until one of the reviewers approves the job.

You can also specify a URL for the environment. The specified URL will appear on the deployments page for the repository (accessed by clicking Environments on the home page of your repository) and in the visualization graph for the workflow run. If a pull request triggered the workflow, the URL is also displayed as a View deployment button in the pull request timeline.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: 
      name: production
      url: https://github.com
    steps:
      - name: deploy
        # ...deployment-specific steps
Deleting an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Next to the environment that you want to delete, click .

Click I understand, delete this environment.

You can also delete environments through the REST API. For more information, see "REST API endpoints for repositories."

How environments relate to deployments
When a workflow job that references an environment runs, it creates a deployment object with the environment property set to the name of your environment. As the workflow progresses, it also creates deployment status objects with the environment property set to the name of your environment, the environment_url property set to the URL for environment (if specified in the workflow), and the state property set to the status of the job.

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "REST API endpoints for repositories," "Objects" (GraphQL API), or "Webhook events and payloads."

Next steps
GitHub Actions provides several features for managing your deployments. For more information, see "Deploying with GitHub Actions."

Press alt+up to activate
Help and support
title: Using environments for deployment
shortTitle: Use environments for deployment
intro: You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.
product: '{% data reusables.gated-features.environments %}'
redirect_from:
  - Request/actions/reference/environments/Repo Approval Request/actions/reference/environments/Repo Approval 
  - /actions/deployment/environments
  - /actions/deployment/using-environments-for-deployment
topics:
  - CD request for digital approval 
  - Deployment
versions:
  fpt: '*'name: Deployment

on: Harmony Blue Branding 
  push: for Request for digital declaration 
    branches: Request for digital approval 
      - main

jobs: Required pre-purchased credit before alternative legacy APIs data entry workload of Merger Workspace layout and services input repo
  deployment:Url location site code maintenance is an legal certification business platform creatively format work flow on Port editing site fix assistance solutions for this primary website to be able to make changes to historical data content could be beneficial or either dangerous for the our business digital system please take in this consideration before adding your input on this business development resources page Thank you 
    runs-on: editor are welcome upon administrative service approvals ubuntu-latest website work-flow information updates are greatly appreciated but please be aware and vigilant of your online asset protocols before editing this incorporation business online website pages
    Please include your the address to your objective and your own itemized information before completing your work flow with this apmis-step is needed first environment: what digital declaration to this incorporation digital declaration of operating add-on production
    steps: what work criteria are you adding to this incorporation digital account 
      - name: please add benefitical to the public library rescription proxy for the public url sites space include the proper connected dedications formation to be completed by you like updating adding on resources to provide help with affiliate management support/input to assistance with resources for references to help with errors or support products fixes/assistance solutions for support products with resources to nonprofit organization fundraising publication before completion or deploy
        # ...deployment-specific steps include the proper regards to the date, time and effort source link to company active legacy APIs Group url location library information for usage status on all the digital programs development and support products that are attached to the business website page and links to this incorporation affiliated workspace entity 
  ghes: '*'
  ghec: '*'
---


## About environments

Environments are used to describe a general deployment target like `production`, `staging`, or `development`. When a {% data variables.product.prodname_actions %} workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

{% ifversion actions-break-glass %}Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."{% endif %}

{% ifversion fpt %}
{% note %}

**Note:** Users with {% data variables.product.prodname_free_user %} plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %} can configure environments for private repositories. For more information, see "[AUTOTITLE](/get-started/learning-about-github/githubs-plans)."

{% endnote %}
{% endif %}

## Deployment protection rules

Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches.{% ifversion actions-custom-deployment-protection-rules-beta %} You can also create and implement custom protection rules powered by {% data variables.product.prodname_github_apps %} to use third-party systems to control deployments referencing environments configured on {% data variables.location.product_location %}.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

{% data reusables.actions.custom-deployment-protection-rules-limits %}

{% endif %}

### Required reviewers

Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

{% ifversion deployments-prevent-self-approval %}You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.{% endif %}

For more information on reviewing jobs that reference an environment with required reviewers, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments)."

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, required reviewers are only available for public repositories.

{% endnote %}{% endif %}

### Wait timer

Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, wait timers are only available for public repositories.

{% endnote %}{% endif %}

### Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}

Use deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} to restrict which branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to the environment. Below are the options for deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} for an environment:

{% ifversion deployment-protections-tag-patterns %}
- **No restriction**: No restriction on which branch or tag can deploy to the environment.
{%- else %}
- **All branches**: All branches in the repository can deploy to the environment.
{%- endif %}
- **Protected branches{% ifversion deployment-protections-tag-patterns %} only{% endif %}**: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "[AUTOTITLE](/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)."{% ifversion actions-protected-branches-restrictions %}

  {% note %}

  **Note:** Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

  {% endnote %}{% endif %}
- **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**: Only branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} that match your specified name patterns can deploy to the environment.

  If you specify `releases/*` as a deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule, only a branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} whose name begins with `releases/` can deploy to the environment. (Wildcard characters will not match `/`. To match branches{% ifversion deployment-protections-tag-patterns %} or tags{% endif %} that begin with `release/` and contain an additional single slash, use `release/*/*`.) If you add `main` as a branch rule, a branch named `main` can also deploy to the environment. For more information about syntax options for deployment branches, see the [Ruby `File.fnmatch` documentation](https://ruby-doc.org/core-2.5.1/File.html#method-c-fnmatch).

  {% ifversion deployment-protections-tag-patterns %}

  {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

  {% endif %}

{% ifversion fpt %}{% note %}

**Note:** Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are also available for private repositories.

{% endnote %}{% endif %}

{% ifversion actions-break-glass %}

### Allow administrators to bypass configured protection rules

By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

{% ifversion fpt %}{% note %}

**Note:** Allowing administrators to bypass protection rules is all available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}
{% endif %}

{% ifversion actions-custom-deployment-protection-rules-beta %}

### Custom deployment protection rules

{% data reusables.actions.custom-deployment-protection-rules-beta-note %}

{% data reusables.actions.about-custom-deployment-protection-rules %} For more information, see "[AUTOTITLE](/actions/deployment/protecting-deployments/creating-custom-deployment-protection-rules)".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "[AUTOTITLE](/actions/deployment/protecting-deployments/configuring-custom-deployment-protection-rules)."

{% ifversion fpt %}{% note %}

**Note:** Custom deployment protection rules are only available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}

{% endif %}

## Environment secrets

Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "[AUTOTITLE](/actions/security-guides/using-secrets-in-github-actions)."

{% ifversion fpt %}
{% note %}

**Notes:**

- Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."
- Environment secrets are only available in public repositories if you are using {% data variables.product.prodname_free_user %}. For access to environment secrets in private or internal repositories, you must use {% data variables.product.prodname_pro %}, {% data variables.product.prodname_team %}, or {% data variables.product.prodname_enterprise %}. For more information on switching your plan, see "[AUTOTITLE](/billing/managing-the-plan-for-your-github-account/upgrading-your-accounts-plan)."

{% endnote %}
{% else %}
{% note %}

**Note:** Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."

{% endnote %}
{% endif %}

## Environment variables

Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[AUTOTITLE](/actions/learn-github-actions/variables)."

{% ifversion fpt %}{% note %}

**Note:** Environment variables are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, environment variables are also available for private repositories.

{% endnote %}{% endif %}

## Creating an environment

{% data reusables.actions.permissions-statement-environment %}

{% ifversion fpt %}
{% note %}

**Notes:**

- Creation of an environment in a private repository is available to organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %}.
- Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.

{% endnote %}
{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
{% data reusables.actions.new-environment %}
{% data reusables.actions.name-environment %}
1. Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "[Required reviewers](#required-reviewers)."
   1. Select **Required reviewers**.
   1. Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
   {% ifversion deployments-prevent-self-approval %}1. Optionally, to prevent users from approving workflows runs that they triggered, select **Prevent self-review**.{% endif %}
   1. Click **Save protection rules**.
1. Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "[Wait timer](#wait-timer)."
   1. Select **Wait timer**.
   1. Enter the number of minutes to wait.
   1. Click **Save protection rules**.
{%- ifversion actions-break-glass %}
1. Optionally, disallow bypassing configured protection rules. For more information, see "[Allow administrators to bypass configured protection rules](#allow-administrators-to-bypass-configured-protection-rules)."
   1. Deselect **Allow administrators to bypass configured protection rules**.
   1. Click **Save protection rules**.
{%- endif %}
{%- ifversion actions-custom-deployment-protection-rules-beta %}
1. Optionally, enable any custom deployment protection rules that have been created with {% data variables.product.prodname_github_apps %}. For more information, see "[Custom deployment protection rules](#custom-deployment-protection-rules)."
   1. Select the custom protection rule you want to enable.
   1. Click **Save protection rules**.
{%- endif %}
1. Optionally, specify what branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to this environment. For more information, see "[Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}](/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches{% ifversion deployment-protections-tag-patterns %}-and-tags{% endif %})."
   1. Select the desired option in the **Deployment branches** dropdown.
   1. If you chose **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**, to add a new rule, click **Add deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule**
   {% ifversion deployment-protections-tag-patterns %}1. In the "Ref type" dropdown menu, depending on what rule you want to apply, click **{% octicon "git-branch" aria-label="The branch icon" %} Branch** or **{% octicon "tag" aria-label="The tag icon" %} Tag**.{% endif %}
   1. Enter the name pattern for the branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} that you want to allow.
      {% ifversion deployment-protections-tag-patterns %}

      {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

      {% endif %}
   1. Click **Add rule**.
1. Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "[Environment secrets](#environment-secrets)."
   1. Under **Environment secrets**, click **Add Secret**.
   1. Enter the secret name.
   1. Enter the secret value.
   1. Click **Add secret**.
1. Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[Environment variables](#environment-variables)."
   1. Under **Environment variables**, click **Add Variable**.
   1. Enter the variable name.
   1. Enter the variable value.
   1. Click **Add variable**.

You can also create and configure environments through the REST API. For more information, see "[AUTOTITLE](/rest/deployments/environments)," "[AUTOTITLE](/rest/actions/secrets)," "[AUTOTITLE](/rest/actions/variables)," and "[AUTOTITLE](/rest/deployments/branch-policies)."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

## Using an environment

Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

{% data reusables.actions.environment-example %}

## Deleting an environment

{% data reusables.actions.permissions-statement-environment %}

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
1. Next to the environment that you want to delete, click {% octicon "trash" aria-label="Delete environment" %}.
1. Click **I understand, delete this environment**.

You can also delete environments through the REST API. For more information, see "[AUTOTITLE](/rest/repos#environments)."

## How environments relate to deployments

{% data reusables.actions.environment-deployment-event %}

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "[AUTOTITLE](/rest/repos#deployments)," "[AUTOTITLE](/graphql/reference/objects#deployment)" (GraphQL API), or "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads#deployment)."

## Next steps

{% data variables.product.prodname_actions %} provides several features for managing your deployments. For more information, see "[AUTOTITLE](/actions/deployment/about-deployments/deploying-with-github-actions)."
Thank you for sharing your online asset with our team workspace for our nonprofit organization website page and site affiliate stems online business sites programs development director layouts for our business website url https and http ID multiple level digital site domain web portal database access point by Google Domain Sites Enterprise of view interface proxy server legacy with Google Business Manager Suite and Google Global maps Proxy Home and special Mediant extinct format digital partnership, Affiliate Marketing applications and services branding promotional content for the public network and multiple companies digital YouTube,Need for Good, Black American Computer Science & Technology analysts Engineering services Beta Developer Business AI Executive AI Executive director layouts and education services for Technology analysts Program Manager Association Manager Advanced marketing services Agency Al Gemini digital legion solutions services for community console and accreditions-campaign services Elite's Expertise Technology analysis data documention official prospects associated registered business platform creatively forum connected with targeted data entry editor for productions to industry support for Google Advertisement Company active legacy APIs Group for Google Global communications projects formation for assessment materials and services grant program engineering youth education projects with resources relations Google database coding Expert and Impact Internal Federal Online Safety forums Interpol EU digital data protections business online affiliate, US Government Historianal Engineer/ GU federal credit historianal engineer authorizations data ancestor publish media operational status Creative Workshop Research Educator/publications of federal laws and development director of digital declaration of operating title developer/media reporter freelancer/Digital media layouts Editors library of admissions resources to document CFR social auditor/Legal Congressional library NIST- Publication editor/author network stems/IRS legacy API proper connected to published work flow data for documentions and publication governmental education business programs/OSHA office operations education business services operations legal program Manager Education business website affiliate marketing branding institutionalized workshop Administrative role in ID access member and ID me digital programs affiliate online network Digital primary contact of admin Publication authorized to provide assistance assessment and support products editor enforcement to provisions of support and services for execution to directory layouts and quote information for publication listings optional-way for educational solutions for business accounts for IDme.gov/Congressional-library .gov and digital CFR.gov publication listings for documention data entry workload and digital support input for provisions forms for Google data questionnaire and email assistance logistics and 
partnership with logistics taskforce advocate & member of investors to investigation on edition Mediant extinct format diverse director and discussion regarding console quote for various resoureful commissions sites business resolution to public website user, business organization and individual requested assistance with resources and information for usage status on legacy data updates forms on the public website regarding review & question for answers to solutions topics for website help pages with support assistant requests. Our business website page and online business are innovative and creatively forum connected to assistance individuals with what ever they may need help with bith on and off line as well as an active member of multiple interactions Digital Health and assistance works for our assistance with the Google brand name for our own itemized form of public assistance and services for this primary caring organization that's fully invested in various types of business affiliated networks/programs / partner/ branding promotional content digital declaration social media creations/affiliate online business marketing/education services and supporting editing skills to learn customized and data entry for different types of community supportive systems tools-Productions-Make alliances all-types of business associate with resources support for any platform or operator division eaudience in input assessment solutions Creative workshop business services sales consultant in the constructively directed streams of public information to help with the the security data and judgement and support assistance with online knowledge of maintaining the proper connected dedications formation for everyone to digital obtain the sublevels of information and services understand and direction and communication accredited knowledge and circumstances educational solutions for their requests to be addressed in a respectful manner and equally with the proper connected information to the public resources and architecture inquire if they're needed understanding of accessibility to reflect, adamantly fix the problem of what ever the quarry was in the constructively directed streams of course offer to exactly direct interaction with the proper instructions for their requests based upon the proper dedications formation reference to the public information to be administrator on behalf of the digital business level particulars while additional access to update internal requirements details assignment direct deposit install instructions for the public data documention and the entitled format of the inquiry output for the publicated directory services methods to provide help with assurance of whatever corporate clarification to be used for the reasonable assetment of input that's would be needed for that individualized record of requestsending unusual form for the most recent/accord information to be delivered for that precise and targeted questions to the proper connected provisions with in the constructively correctiodesigned material for asset management of misrepresented overall consistency reputation for the companies integrity, safety, and legitimate policy's and general guidelines of the digital business online regulated language and procalls to be upheld in regards to their own requirements for this online business website page digital declaration invitation for the next step of course the addressing the proper infornative about input to available to choose the most effective beneficently revision term of agency aggressive interest in obtaining the best opportunity for investing the public with the multi factor investing knowledge for the duty infinite information and services site fix with resources to two or more option available to choose from this position offers the customer to be able to create an secondary opinion on different methods to try to be able to gain control and access on trying to accord complete whatever issues that maybe an issued response to the file transfer resources to provide help with the errors progressive data entry inquiries to this documented incident report for the unlimited resources to information rely required to commotions accreditions-campaign really reputable to the public question for assistance with resources by the intended source link and data accommodations for the proposal application services trouble shooting resolved encounter with the reliable programs data connection with donations inquiry to be answered with the device instead step in calculated reliability means for the solutions to the topic of candidates console services unconditionally contact of interest of content containing solutions that are attached to and relevant to the question of the topic of each individual request for digital assistance with resources requests for the most recent a certain quality dedicated rebutionals to production and services includion data recovery information to deverepeate/offer platforms advice for the total reference to help execute and supportive measures for optimum instruction of accurate understanding step by step with the instructions in language that innovatively understandable to the average individual's for the results information layouts in a full clear and content to betterment that most people can combative comprehend the digital resources instructive formation method for everyday adults with little of no interest in learning computer laggo or logistics writings for comparison of repost ID reactivate compatible computer science technology position instructions vs regular function format language detail on how to use or fix errors and computer updates for delivery reposition to accommodate a really teachable upstanding to help with the desired format to offer fully data and easily flow written individuals information for a more reliable user experience with resources and wanted to share interest succession of gainful interactions, and more direct hand on usages for that and more data products than can be produced and purchased by the intended cusmoter for the extra revolution for the public health od public perception of this primary company active in business sales and longevity status with the proper connected dedications for the understanding of accessibility to reflect materials that the average person can understand and use affirmatively daily or either assessment of assets to provide assistance with needed resources for the addition public prosecutor to be conserved effect on efforts to make life decisions for a easier relationship with building the public knowledge of useful content and self services understanding of accessibility to reflect on the values of that rebrand realistic resolution for the proper connected educations formation of the digital material and essential facts about self service support for future reference to help with error fixes that gainful profitable meaningful Internally investment in regards to resourcful methods to provide assistance with the proper easy understand reasonable assets for the self confidence in be able to make adjustments for the individualize ability to provide self Care Provider for ones self by providing the proper multipurpose operational services steps in regards to this one 
request for digital programs assistance with the device and digital programs products that are directional compatible with the proper steps by step instructions and in the information landscape of language that's clear and precise to accurately address concerns the proper regards request of assistance with or either about a products or services request for digital programs development director layouts and education for instructions on how to officially operate the proper the digital products that maybe needed at that point for resources and details instructions assessments issues. This organization has always had the best and most meaningful interest in privileged in regards to the public delivery of service data accessment in regards to assisting individuals with the proper connected educations formation for individuals to become properly informated with the necessties tools, data, institutional related instructions, transactional status recording details, trouble shooting methods to respectfully and written in regards to underable language steps to the provider proper understandable response to the public logistics of the contractly regarding methods of direct data recovery and usage instructions for with an update most openings for incorprative formcreatively directoryhandout for director full compatible comprehension for required edition information to upon the public requested for real situation inquiry to provision's of addressing the proper dedications formation for the issues of connection questions and answers of assistance with resources for references needed to be completed by development director assistance developers for Google blogs and community services representative that are available to help with any type of additionalitemized litemized for asset management of material on the public website that are available to help with individuals related in the constructively directed work flow assessment of the digital produces and services protocols for assistance with resources to provisions of items problems with the proper connected safety and policies for associated historically corporate clarification policies and rules of the digital platform for the users regulated language in the constructively correctiodesigned material form of with the decorative of the digital business online website and developer freelance platforms regulated language and data support systems tools-Productions-Make alliances all-types of concerns to the business request formats and editing delivery design database affidavit admissions to the primary care providers mastering estimated assistance with the device for the time of the demographic that's related to the public register for handling the digital programs development editor and educational solutions workspaces/entitled digital programs fields of admissions to this incorporation affiliated workspace title of the digital coding and encompass business programmer management reserve required networks dedications of reasonables /market place in regards to Google platform creative Merchant  online business sites Collaboration Account Manager workshop Administrative connected with targeted Digital Primary programmers scholarship to provisions by Microsoft Data Business Relations Marketing LLC online business sites space for Entity and financial service memberships. Dedications of admissions to Harmony Blue Branding Organization Business Group for Google industries & affiliate online services. This organization assistment digital declaration communities in the constructively directed proper composition of the digital business online website work-flow information.The professionalism was directory targeted focused groups. Aligned for our relatives platform and the whole founded objective as an financial needed for govnen revenue goal for Supportive interest and equipment services for the needed resources and services fees goal of the digital business financially funding ongoing income for the public emerged infrastructure security both digitally and pumpkin to be provided effective benefits of ensured for the public sector that every single young individuals has been prevented with the proper connected dedications formation for the entire consult of the digital programs development and educational services, data access to cover the wireless work location, the proper number of devices and equipment for business services and supporting resourceful supplies for operations legal rights for the next few months of digital Business of more digital devices ti the property location and service representative office operations manager to help with growth and support several types of resoureful products and digital products software for future contact servers modeled to provide assistance with the best opportunity for the young individuals to succeed and mobility follow through the internal templates of information without being offset with digital logically issues, digital proforms database speed for a crosspoint platform router with resources to support a of 20 services device service all as once.The desire digital devices that have an operational status title to fighting history of becoming jetlack and development issues with the performance connection issues of jinky data access on site uploads to slow down after an few weeks even with resources to provide assistance for consideration time duration to resl time to automatic logout reload to the home website work-flow format even with the proper connected dedications formation of self proved to model to reset  focused hardware for the public safety of privacy data properly connected with the best suitable amount of firewall services equipment. poorly performing computers and devices still are known to becoming problematic with an short span of time with the daily  used forum and including the recognized digital entry of technology projects and designs for anyone who is using the property directly from the tribe source related fields of unlimited uploading and downloading materials that are associated with the device drive root redirect to the function settings and this is so even with resources of saving the proper connected resources to an extent thumb drive as well. I've been extremely fortunate to receive offers for products at a discount business price but the conditions based on the price of the bulk product did not meet and will not meet my expectations including them being refurbished it's also additional Factor business conditions of course of this months of this primary needed contributions and resources entity properties for development director layouts and education services for the public youth program reliability means for the results of the same the primary consideration of the digital business level of connection functionality of the digital business online demand security equipmentinterest in provisions of comparisons services for the public online business official prospects production of the Harmony Blue Branding LLC online business sites space in the incorporate status of the Publication records accredited nonprofit organization for the Harmony Blue, the digital business connection to synced detail to corporate clarification to the public forum for the Harmony Blue Foundation and unique classification for the Harmony House Foundation assets provide assistance with resources of additional details reference to reflect the public executive operations legal certification federal credit union of the Harmony Blue index as an multiple increments of the digital business online website work-flow merger page for qualified directly exempt status with the proper dedications standards of admissions resources this nonprofit organization legal verification records forms on network with the Secretary of state certified business office operations manager Owner Founder Cheyanna Henry and solely partnership with public records and license documentions records for sole business claiming to and for legalize owner of public reconciliation in the official prospects of associated registered business with both federal and state judication for legal administrative corporate direct access deposit data approval for operations status with the United States registered for legal operations for business services and operations services of corporate clarification business government office for registration regulations required to commotions accreditions-campaign with the BBB and certification business verification of public records by Yelp and Google Business operations with good legal rights to active declaration of operating title of Administrator grade A+ standings. Legacy financial ownership over all of alliance titles that are attached to the public business services operations legal rights to law federal government services based upon regulations required the proper connected position with resources and public data access records for linked to the business process records forms on relisted and financial filing requirements needed for this primary position to be with in the constructively directed format of the digital business online state of justify jurisdiction records forms on iexceptions provisions with no notarized expiration date of service for social revision to be ablity or applied for this primary business organization and business design database Group for prevention of the digital programs and architecture health to services associated registered to and publication owner transfer design  trademarked to be delivered in relation to this corporative and incorporation business organization services.All services supportive and professional submission for the publication inquiries for and about this and any other details information on operational status and digital declaration library of services.Are on my website work-flow merger page for qualified directly to the public records forms on networking and content communication accredited to this incorporation affiliated workspace title Account Manager and services for the public enlightened to this incorporation digital programs and development projects, foundation services and supporting resoureful health services and operations management community project design database information for usage status on legacy APIs Group chat or organization fundraising publication of the digital business online information for services. Reminder to be available for future reference to this incorporation business online information would be dedications and migration file folders to the url website work-flow merger pages of this primary position in caring with resources to nonprofit organization for the public view of the digital progressive information to be attached to the public historical website work-flow dashboard proxy server Domain and listed as correspondent for the public records forms legacy knowledge page for qualified directly on the values proper connected to the public dedications formation for the online businesses.All legal certification for the public libraries of interest in various types of products and digital services for the business response to this incorporation affiliated workspace title are on the public library url website website pages for this proxy servers. Observation about the benefitical of the digital business online website work-flow and business organization services mergers to the business development resources to nonprofit organization for the public development resources to provide assistance with the public this encoded within this website sites drive folders and digital historianal engineer authorizations in regards to declaration date and data entries to provided effective benefits are in the work spaces for this primary website primer proxy server legacy APIs website. All division of course audience of all three years of services are incorporated into the digital programs website digital pages of this primary URL multi-level link for this online campaign analytical publications as high 87% know legally operations of course of 2 or more years of services and operations legal rights to owner of office annual budgets for this business and finance services related information for the public resources records reflect materials that are attached and completed recorded for the business cab be found on this business data digital proxy server legacy APIs https openings and http openings and the whole website platform workspaces for historical content for visual recognition of the public library of admissions to various types of resoureful community projections to campaign analytical virtual public market contact in regards to revenue agency Dedications of admissions to various project design database information for this primary program to the business operations of publication of services and supporting resoureful health and youth community project and community effort to assistance with the proper dedications formation of the financial support and diversity development director layouts and education services for the public youth community services projects analytics promotional content ideas on custom crafts resources to nonprofit organization and business management program engineering department promoter and the whole website in traditional company active legacy APIs program engineering services for claiming methods to respectfully writing of during of works for grant writing projections for grant funding proposal abd application listed dayes of applied for real literacy commissions prospects production of abilities for achievvmultiple documents grant funding for services assistant director resources to help with nonprofit organization needs and resources requirements for professional provisions items for comparison of quality assistance to thrive property management preparation for proposals require to apply for existing administrators economical successfully following platform creatively forum connected synced targeted Digital Health assistant associate with resources to nonprofit organization and business foundation services for the public needs to accommodate the proper connected resources to provide help with growth of the companies abilities for the original particulars network online goals in achieved affection for the fundraising publication financial support of expansion efforts for resources to help more individuals, the proper functions and digital support for future devices and services equipment needed to be able to meet the public needs for access to relevant access to multiple dynamic digital programs and urgent support products to the web portal database information for usage of official prospects in regards to the business response for the children of the local communities for the device assistance with optional costs for the equipment and office operations supplies and offices functional company desktop office operations legal solutions products including the publication business services office furnitures for the proper connected dedications formaled item's request to provide the proper access to the business network digital programs for the public education programs development director of digital programs and support development resources for the proper dedicated hopeful abilities for achievements are the best opportunity to gainful commending for our youths and family services programs. Due to si many unrealistic situations in and with this apmis-step on this business data access to existing administrator assistance with the proper regards to the prixy servers legacy APIs access point of services and operations output linked database for the business page for this primary website always degraging to regularly payloads server Domain IP links to the business director digital business online website connected repetatively resource communication projections to request fix fork and code maintenance for the public url lodge on the public library database information to register domain sites and hist submit support site location for this online business to be always conversing to having static data issues daily with without any type of additionalitemized litemized reasons for this online related to repeatly be happening so frequently between every 6 to 9 months reaction time during the time of the very needed resources submission to application process for financial assistance request for business services operations resources funding grant submittions for the business needs of assistance with resources to the non-profit organization primary assistance with resources to help with principles projects.Digital issues surrounding this primary problem resourced in the self eduational knowledge of the digital respectful needs to be ables to essential expert knowledge and analysis skills to learn every single team member learning how to prioritize the proper digital programs development education business services operations specialist and sales Special edition for the public sector in regards to conpter science technology engineering and engineering logistics software management skills for the custom crafted resources to assist with the proper dedications provisions of advanced releases to products exist respect for developmentprerelease beta affiliate corporate sponsorship executive director layouts and education business management support systems tools-Productions-Make alliances openings for branding commission analysis company market for exterior demolition of primary test kit before release date of service software and Digital website work-flow connection with top category criteria corporation with in the technology fiejds and the top confirmed development software and firmware company market logistics testing performance pre-release for prompt entity properties trailer to use for limited capacity for testing of their system products before the publication launch date. I have the best experience with the most of thud typr of offers to from this organization collected collaborative branding promotional content program edition and merging with the faculty team contributions accessibly of the digital programs and signature corporations like IBM, Digital icon Douglas navg. software for mathematics and digital device technology and development, software engineer, authorizations data T-Mobile Wireless  Opera, Microsoft Data content Business workshop Administrative corporate sponsorship program for various resources and grant business website resources, Google docs data access to studio sponsorship for YouTube creative publication contract for Google industries,etc...the list extinct is too long to be able to review alliance various types of products affiliated with the proper regards to special digital distribution for the best opportunity for this online business website services to required online business sites and data access for the program execution to directory layouts itemized formal exceptions provisions items as resources network logistics to requested resources to nonprofit organization and business organization services development teams to provide assistance with the unlimited and excellent customer relations professionals dream of being able to achieve the vision obtained operations over the course of this business operations legacy of business associate director of education services for the public education business management relationships in regards to to active Dedications leadership and learning resources for expanding knowledge of maintaining itemized formal exceptions provisions of comparisons services representative of tenant executive director digital programs development education skills essential on the public value existing administrators economically successfully as well the proper related features are needed for the digital programs and achievements directly to depositing input and output data website work-flow information for future configuration and executive director design database coding Expert services for claiming methods engineering reliable programs safe for folk and report various types of rep repositories of data matrix and external factors platforms of operational status on puls format director layouts in Editorial direct access to multiple resources to reverse and revise coding error for fix assistance solutions about benefitical test switch to provide help with sight solutions of innovation directory layouts itemized reference format dymierrors category individualized recording on the values proper connected dedications formation for the public estimated assistance in the constructively streams in regards to the business response to the plugs and the drive in public library of course the title verify auto correct information for usage status on the public health administration services request for digital programs development assistant Lead to promotion self support products for seeking various edit ms data portal program engineering and workload source link acodes for the necessary details tgat ate needed to accommodate the digital coding trouble shooting content for overall consistency ratio of an authorized execution of yermsk pass through documents shell and the whole needs website fault resources runs to provide the proper connected plug ti to be attached to the public library or file information for usage status correct designed to provide direct dig requirements to the intended rebutionals errors shorten and runs to the pull test for the next debug submit to the input genuine supportive code sites link support products for the next and steps in regards to services true and finally pass transportation to the point of request for digital programs come gainful profitable meaningful Internally to be effective to help with support products for web portal Repo in regards to the website resource link to seek for historical reviews real time health to this work flow data documention and refigured changes to the outline output of the digital programs development of the foundation of relationship knowledge of the digital programs and availability for obtaining additional information and educational services in with the current specialisted field of study in computer science and digital science technology analysis of to respectfully accomplish the proper regards to complete call my ab respectable engineer and at least obtain the ultra development skills for custom crafts and forever evolving tr trending fields in regards to digital programs and safety data analytics experience with the proper regards to tine and structure study in the duration or probably 3more to 5 more years plus recent My authorizations data certificate for the relation to the understanding of every single available resources learning programs and educational skills for custom crafts incentives of one day becoming a real estate agent with the proper dedications certifications and licences  required to be completely honored to be considered as an experienced digital engineering and professional programs development in software design and support business management skill publication connected to the website renewal portal workspace title database coding effectively meet the recognitions requirements needed to obtain the ultra high quality dedicated for everyone to be able to obtain.The  team and me the principal and somewhat their platform teacher and motivation support for them to learn and own the ability to build up the proper infrastructure understanding with the best opportunity to reach this reliability programs goal needed to be conserved effective for asset with resources and data entry Firm in the constructively directed streams of capable abilities to provide assistance with the proper environment to assist with financial support, education editing writing experience with digital programs networks for social media marketing executive operations legion solutions services technology project design database coding system with development publication listings recognized skills set based upon the public team learning group editing videos and exercises for this primary position to hopefully get better and understanding of accessibility to reflect material for various types of business administration resources to nonprofit organization and business organization services assistance representative of corporate operations manager to help access with candidates to acknowledge that required pre-purchased studies experience to lecture with the device tools with Microsoft Data content forms to create an connection updated skills to custom crafts and received the proper dedications formation for obtaining quality team contributions accessibly in every single person ability to test for the public titles and recipient joint submission application for the next step in service requirements for this primary term in regards to owner coverage for the publication testing process for recipient estimated purchasing for each team member to be able to obtain their own personal licenses for the required field and experiences of study as listings for receiving the public records forms for benefitical certification business license titled of course studies course associated registered business Digital computer science technology development of corporate clarification professional provisions for themselves and executive directly to their own personal employment resume for the achievement official prospective titled associated registered business services operations The dedication of value in legal rights to be able to gain the proper certification in business and finance services sales Special Mediant list form of certified title, professional associated degree in business management of legal data recording, direct access to multiple resources online to verification of products regulations corporate network registration for audit digital programs development in virtual license business services operations and tax experience finance soes list extinct with the IRS letter offical approval for this online legacy of business associate course. For nonprofit organization members that are associated with this organization works and daily practices.This programs for architecture require to the public as an optional need education business to help with growth and development succeed offer succession of gainful estimated knowledge for the online class entrepreneurs and certification tools for tax accountant in legal rights to business workers in operational statues of becoming a real licensed tax advocate professional associated with the United States supportive assistance with resources to help with education business management of the operator of the foundation financial advisor and resources to provide internal revenue agency assistance with an internal security data filing taxes for the public citizens of America and the tax paying company that are in need of help with processing and preparing tax records online. When the public and legal forms position to receive connected commentary compensation for office operations legal filing assistance with resources for references. All services provider for the proper connected costs for helping team members the digital testing fees. This organization has been through so many different types of extremely struggling and economic setbacks and imagined data access challenging for this company first 2 years of services. As an understanding and relevant development heartbreaking situations for the public sector in the past and a bunch of other side issues that are presented to the business still to this date. We're outstandingly advocating for and on behalf of anyone who has been extremely dedicated to help with the volunteer services and operations support for the public community services payment from the public are just a well acknowledged Thank you for being their for the connection hands on work and within the public district ministry works with us and in commitment and time to help with making successful decisions on small community services projects and assisting us by recite the public community about our relationship knowledge and communication skills for creating ideas for public relations to help with touching the proper people who have and have been in or are actively going through homeless and other financial difficulties or essential facts experts dur to health care and documented psychological issues. Our primary organization services targeted construction missionaries of services and extremely dire  awareness custom growth downtown areas changes to the public library of admissions resources to educational solutions adviser and services graduated in the constructively corrections designed course materials that related to passed the digital certificate testing.This online experiences with the trust federal revenue agencies in the US educational programs, and online curriculums to the  educational solutions for this one or two listed website public courses. 
e public courses. 
---deployment target like production, staging, or development. When a GitHub ActionsGitHub Actions/Deployment/Target different environments/Use environments for deployment
Using environments for deployment
You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.

Who can use this feature?
Environments, environment secrets, and deployment protection rules are available in public repositories for all current GitHub plans. They are not available on legacy plans, such as Bronze, Silver, or Gold. For access to environments, environment secrets, and deployment branches in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, other deployment protection rules, such as a wait timer or required reviewers, are only available for public repositories.

In this article
About environments
Deployment protection rules
Environment secrets
Environment variables
Creating an environment
Using an environment
Deleting an environment
How environments relate to deployments
Next steps
About environments
Environments are used to describe a general deployment target like production, staging, or development. When a GitHub Actions workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "Viewing deployment history."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "Reviewing deployments."

Note: Users with GitHub Free plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with GitHub Team and users with GitHub Pro can configure environments for private repositories. For more information, see "GitHub’s plans."

Deployment protection rules
Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches. You can also create and implement custom protection rules powered by GitHub Apps to use third-party systems to control deployments referencing environments configured on GitHub.com.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

Note: Any number of GitHub Apps-based deployment protection rules can be installed on a repository. However, a maximum of 6 deployment protection rules can be enabled on any environment at the same time.

Required reviewers
Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.

For more information on reviewing jobs that reference an environment with required reviewers, see "Reviewing deployments."

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, required reviewers are only available for public repositories.

Wait timer
Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, wait timers are only available for public repositories.

Deployment branches and tags
Use deployment branches and tags to restrict which branches and tags can deploy to the environment. Below are the options for deployment branches and tags for an environment:

No restriction: No restriction on which branch or tag can deploy to the environment.

Protected branches only: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "About protected branches."

Note: Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

Selected branches and tags: Only branches and tags that match your specified name patterns can deploy to the environment.

If you specify releases/* as a deployment branch or tag rule, only a branch or tag whose name begins with releases/ can deploy to the environment. (Wildcard characters will not match /. To match branches or tags that begin with release/ and contain an additional single slash, use release/*/*.) If you add main as a branch rule, a branch named main can also deploy to the environment. For more information about syntax options for deployment branches, see the Ruby File.fnmatch documentation.

Note: Name patterns must be configured for branches or tags individually.

Note: Deployment branches and tags are available for all public repositories. For users on GitHub Pro or GitHub Team plans, deployment branches and tags are also available for private repositories.

Allow administrators to bypass configured protection rules
By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "Reviewing deployments."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

Note: Allowing administrators to bypass protection rules is only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Custom deployment protection rules
Note: Custom deployment protection rules are currently in public beta and subject to change.

You can enable your own custom protection rules to gate deployments with third-party services. For example, you can use services such as Datadog, Honeycomb, and ServiceNow to provide automated approvals for deployments to GitHub.com. For more information, see "Creating custom deployment protection rules".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "Configuring custom deployment protection rules."

Note: Custom deployment protection rules are only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Environment secrets
Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "Using secrets in GitHub Actions."

Notes:

Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "Security hardening for GitHub Actions."
Environment secrets are only available in public repositories if you are using GitHub Free. For access to environment secrets in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. For more information on switching your plan, see "Upgrading your account's plan."
Environment variables
Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the vars context. For more information, see "Variables."

Note: Environment variables are available for all public repositories. For users on GitHub Pro or GitHub Team plans, environment variables are also available for private repositories.

Creating an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Notes:

Creation of an environment in a private repository is available to organizations with GitHub Team and users with GitHub Pro.
Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.
On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Click New environment.

Enter a name for the environment, then click Configure environment. Environment names are not case sensitive. An environment name may not exceed 255 characters and must be unique within the repository.

Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "Required reviewers."

Select Required reviewers.
Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
Optionally, to prevent users from approving workflows runs that they triggered, select Prevent self-review.
Click Save protection rules.
Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "Wait timer."

Select Wait timer.
Enter the number of minutes to wait.
Click Save protection rules.
Optionally, disallow bypassing configured protection rules. For more information, see "Allow administrators to bypass configured protection rules."

Deselect Allow administrators to bypass configured protection rules.
Click Save protection rules.
Optionally, enable any custom deployment protection rules that have been created with GitHub Apps. For more information, see "Custom deployment protection rules."

Select the custom protection rule you want to enable.
Click Save protection rules.
Optionally, specify what branches and tags can deploy to this environment. For more information, see "Deployment branches and tags."

Select the desired option in the Deployment branches dropdown.

If you chose Selected branches and tags, to add a new rule, click Add deployment branch or tag rule

In the "Ref type" dropdown menu, depending on what rule you want to apply, click  Branch or  Tag.

Enter the name pattern for the branch or tag that you want to allow.

Note: Name patterns must be configured for branches or tags individually.

Click Add rule.

Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "Environment secrets."

Under Environment secrets, click Add Secret.
Enter the secret name.
Enter the secret value.
Click Add secret.
Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the vars context. For more information, see "Environment variables."

Under Environment variables, click Add Variable.
Enter the variable name.
Enter the variable value.
Click Add variable.
You can also create and configure environments through the REST API. For more information, see "REST API endpoints for deployment environments," "REST API endpoints for GitHub Actions Secrets," "REST API endpoints for GitHub Actions variables," and "REST API endpoints for deployment branch policies."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

Using an environment
Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "Viewing deployment history."

You can specify an environment for each job in your workflow. To do so, add a jobs.<job_id>.environment key followed by the name of the environment.

For example, this workflow will use an environment called production.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: deploy
        # ...deployment-specific steps
When the above workflow runs, the deployment job will be subject to any rules configured for the production environment. For example, if the environment requires reviewers, the job will pause until one of the reviewers approves the job.

You can also specify a URL for the environment. The specified URL will appear on the deployments page for the repository (accessed by clicking Environments on the home page of your repository) and in the visualization graph for the workflow run. If a pull request triggered the workflow, the URL is also displayed as a View deployment button in the pull request timeline.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: 
      name: production
      url: https://github.com
    steps:
      - name: deploy
        # ...deployment-specific steps
Deleting an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Next to the environment that you want to delete, click .

Click I understand, delete this environment.

You can also delete environments through the REST API. For more information, see "REST API endpoints for repositories."

How environments relate to deployments
When a workflow job that references an environment runs, it creates a deployment object with the environment property set to the name of your environment. As the workflow progresses, it also creates deployment status objects with the environment property set to the name of your environment, the environment_url property set to the URL for environment (if specified in the workflow), and the state property set to the status of the job.

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "REST API endpoints for repositories," "Objects" (GraphQL API), or "Webhook events and payloads."

Next steps
GitHub Actions provides several features for managing your deployments. For more information, see "Deploying with GitHub Actions."

Press alt+up to activate
Help and support
title: Using environments for deployment
shortTitle: Use environments for deployment
intro: You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.
product: '{% data reusables.gated-features.environments %}'
redirect_from:
  - Request/actions/reference/environments/Repo Approval Request/actions/reference/environments/Repo Approval 
  - /actions/deployment/environments
  - /actions/deployment/using-environments-for-deployment
topics:
  - CD request for digital approval 
  - Deployment
versions:
  fpt: '*'name: Deployment

on: Harmony Blue Branding 
  push: for Request for digital declaration 
    branches: Request for digital approval 
      - main

jobs: Required pre-purchased credit before alternative legacy APIs data entry workload of Merger Workspace layout and services input repo
  deployment:Url location site code maintenance is an legal certification business platform creatively format work flow on Port editing site fix assistance solutions for this primary website to be able to make changes to historical data content could be beneficial or either dangerous for the our business digital system please take in this consideration before adding your input on this business development resources page Thank you 
    runs-on: editor are welcome upon administrative service approvals ubuntu-latest website work-flow information updates are greatly appreciated but please be aware and vigilant of your online asset protocols before editing this incorporation business online website pages
    Please include your the address to your objective and your own itemized information before completing your work flow with this apmis-step is needed first environment: what digital declaration to this incorporation digital declaration of operating add-on production
    steps: what work criteria are you adding to this incorporation digital account 
      - name: please add benefitical to the public library rescription proxy for the public url sites space include the proper connected dedications formation to be completed by you like updating adding on resources to provide help with affiliate management support/input to assistance with resources for references to help with errors or support products fixes/assistance solutions for support products with resources to nonprofit organization fundraising publication before completion or deploy
        # ...deployment-specific steps include the proper regards to the date, time and effort source link to company active legacy APIs Group url location library information for usage status on all the digital programs development and support products that are attached to the business website page and links to this incorporation affiliated workspace entity 
  ghes: '*'
  ghec: '*'
---


## About environments

Environments are used to describe a general deployment target like `production`, `staging`, or `development`. When a {% data variables.product.prodname_actions %} workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

{% ifversion actions-break-glass %}Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."{% endif %}

{% ifversion fpt %}
{% note %}

**Note:** Users with {% data variables.product.prodname_free_user %} plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %} can configure environments for private repositories. For more information, see "[AUTOTITLE](/get-started/learning-about-github/githubs-plans)."

{% endnote %}
{% endif %}

## Deployment protection rules

Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches.{% ifversion actions-custom-deployment-protection-rules-beta %} You can also create and implement custom protection rules powered by {% data variables.product.prodname_github_apps %} to use third-party systems to control deployments referencing environments configured on {% data variables.location.product_location %}.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

{% data reusables.actions.custom-deployment-protection-rules-limits %}

{% endif %}

### Required reviewers

Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

{% ifversion deployments-prevent-self-approval %}You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.{% endif %}

For more information on reviewing jobs that reference an environment with required reviewers, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments)."

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, required reviewers are only available for public repositories.

{% endnote %}{% endif %}

### Wait timer

Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, wait timers are only available for public repositories.

{% endnote %}{% endif %}

### Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}

Use deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} to restrict which branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to the environment. Below are the options for deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} for an environment:

{% ifversion deployment-protections-tag-patterns %}
- **No restriction**: No restriction on which branch or tag can deploy to the environment.
{%- else %}
- **All branches**: All branches in the repository can deploy to the environment.
{%- endif %}
- **Protected branches{% ifversion deployment-protections-tag-patterns %} only{% endif %}**: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "[AUTOTITLE](/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)."{% ifversion actions-protected-branches-restrictions %}

  {% note %}

  **Note:** Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

  {% endnote %}{% endif %}
- **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**: Only branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} that match your specified name patterns can deploy to the environment.

  If you specify `releases/*` as a deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule, only a branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} whose name begins with `releases/` can deploy to the environment. (Wildcard characters will not match `/`. To match branches{% ifversion deployment-protections-tag-patterns %} or tags{% endif %} that begin with `release/` and contain an additional single slash, use `release/*/*`.) If you add `main` as a branch rule, a branch named `main` can also deploy to the environment. For more information about syntax options for deployment branches, see the [Ruby `File.fnmatch` documentation](https://ruby-doc.org/core-2.5.1/File.html#method-c-fnmatch).

  {% ifversion deployment-protections-tag-patterns %}

  {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

  {% endif %}

{% ifversion fpt %}{% note %}

**Note:** Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are also available for private repositories.

{% endnote %}{% endif %}

{% ifversion actions-break-glass %}

### Allow administrators to bypass configured protection rules

By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

{% ifversion fpt %}{% note %}

**Note:** Allowing administrators to bypass protection rules is all available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}
{% endif %}

{% ifversion actions-custom-deployment-protection-rules-beta %}

### Custom deployment protection rules

{% data reusables.actions.custom-deployment-protection-rules-beta-note %}

{% data reusables.actions.about-custom-deployment-protection-rules %} For more information, see "[AUTOTITLE](/actions/deployment/protecting-deployments/creating-custom-deployment-protection-rules)".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "[AUTOTITLE](/actions/deployment/protecting-deployments/configuring-custom-deployment-protection-rules)."

{% ifversion fpt %}{% note %}

**Note:** Custom deployment protection rules are only available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}

{% endif %}

## Environment secrets

Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "[AUTOTITLE](/actions/security-guides/using-secrets-in-github-actions)."

{% ifversion fpt %}
{% note %}

**Notes:**

- Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."
- Environment secrets are only available in public repositories if you are using {% data variables.product.prodname_free_user %}. For access to environment secrets in private or internal repositories, you must use {% data variables.product.prodname_pro %}, {% data variables.product.prodname_team %}, or {% data variables.product.prodname_enterprise %}. For more information on switching your plan, see "[AUTOTITLE](/billing/managing-the-plan-for-your-github-account/upgrading-your-accounts-plan)."

{% endnote %}
{% else %}
{% note %}

**Note:** Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."

{% endnote %}
{% endif %}

## Environment variables

Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[AUTOTITLE](/actions/learn-github-actions/variables)."

{% ifversion fpt %}{% note %}

**Note:** Environment variables are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, environment variables are also available for private repositories.

{% endnote %}{% endif %}

## Creating an environment

{% data reusables.actions.permissions-statement-environment %}

{% ifversion fpt %}
{% note %}

**Notes:**

- Creation of an environment in a private repository is available to organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %}.
- Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.

{% endnote %}
{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
{% data reusables.actions.new-environment %}
{% data reusables.actions.name-environment %}
1. Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "[Required reviewers](#required-reviewers)."
   1. Select **Required reviewers**.
   1. Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
   {% ifversion deployments-prevent-self-approval %}1. Optionally, to prevent users from approving workflows runs that they triggered, select **Prevent self-review**.{% endif %}
   1. Click **Save protection rules**.
1. Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "[Wait timer](#wait-timer)."
   1. Select **Wait timer**.
   1. Enter the number of minutes to wait.
   1. Click **Save protection rules**.
{%- ifversion actions-break-glass %}
1. Optionally, disallow bypassing configured protection rules. For more information, see "[Allow administrators to bypass configured protection rules](#allow-administrators-to-bypass-configured-protection-rules)."
   1. Deselect **Allow administrators to bypass configured protection rules**.
   1. Click **Save protection rules**.
{%- endif %}
{%- ifversion actions-custom-deployment-protection-rules-beta %}
1. Optionally, enable any custom deployment protection rules that have been created with {% data variables.product.prodname_github_apps %}. For more information, see "[Custom deployment protection rules](#custom-deployment-protection-rules)."
   1. Select the custom protection rule you want to enable.
   1. Click **Save protection rules**.
{%- endif %}
1. Optionally, specify what branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to this environment. For more information, see "[Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}](/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches{% ifversion deployment-protections-tag-patterns %}-and-tags{% endif %})."
   1. Select the desired option in the **Deployment branches** dropdown.
   1. If you chose **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**, to add a new rule, click **Add deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule**
   {% ifversion deployment-protections-tag-patterns %}1. In the "Ref type" dropdown menu, depending on what rule you want to apply, click **{% octicon "git-branch" aria-label="The branch icon" %} Branch** or **{% octicon "tag" aria-label="The tag icon" %} Tag**.{% endif %}
   1. Enter the name pattern for the branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} that you want to allow.
      {% ifversion deployment-protections-tag-patterns %}

      {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

      {% endif %}
   1. Click **Add rule**.
1. Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "[Environment secrets](#environment-secrets)."
   1. Under **Environment secrets**, click **Add Secret**.
   1. Enter the secret name.
   1. Enter the secret value.
   1. Click **Add secret**.
1. Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[Environment variables](#environment-variables)."
   1. Under **Environment variables**, click **Add Variable**.
   1. Enter the variable name.
   1. Enter the variable value.
   1. Click **Add variable**.

You can also create and configure environments through the REST API. For more information, see "[AUTOTITLE](/rest/deployments/environments)," "[AUTOTITLE](/rest/actions/secrets)," "[AUTOTITLE](/rest/actions/variables)," and "[AUTOTITLE](/rest/deployments/branch-policies)."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

## Using an environment

Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

{% data reusables.actions.environment-example %}

## Deleting an environment

{% data reusables.actions.permissions-statement-environment %}

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
1. Next to the environment that you want to delete, click {% octicon "trash" aria-label="Delete environment" %}.
1. Click **I understand, delete this environment**.

You can also delete environments through the REST API. For more information, see "[AUTOTITLE](/rest/repos#environments)."

## How environments relate to deployments

{% data reusables.actions.environment-deployment-event %}

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "[AUTOTITLE](/rest/repos#deployments)," "[AUTOTITLE](/graphql/reference/objects#deployment)" (GraphQL API), or "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads#deployment)."

## Next steps

{% data variables.product.prodname_actions %} provides several features for managing your deployments. For more information, see "[AUTOTITLE](/actions/deployment/about-deployments/deploying-with-github-actions)."
Thank you for sharing your online asset with our team workspace for our nonprofit organization website page and site affiliate stems online business sites programs development director layouts for our business website url https and http ID multiple level digital site domain web portal database access point by Google Domain Sites Enterprise of view interface proxy server legacy with Google Business Manager Suite and Google Global maps Proxy Home and special Mediant extinct format digital partnership, Affiliate Marketing applications and services branding promotional content for the public network and multiple companies digital YouTube,Need for Good, Black American Computer Science & Technology analysts Engineering services Beta Developer Business AI Executive AI Executive director layouts and education services for Technology analysts Program Manager Association Manager Advanced marketing services Agency Al Gemini digital legion solutions services for community console and accreditions-campaign services Elite's Expertise Technology analysis data documention official prospects associated registered business platform creatively forum connected with targeted data entry editor for productions to industry support for Google Advertisement Company active legacy APIs Group for Google Global communications projects formation for assessment materials and services grant program engineering youth education projects with resources relations Google database coding Expert and Impact Internal Federal Online Safety forums Interpol EU digital data protections business online affiliate, US Government Historianal Engineer/ GU federal credit historianal engineer authorizations data ancestor publish media operational status Creative Workshop Research Educator/publications of federal laws and development director of digital declaration of operating title developer/media reporter freelancer/Digital media layouts Editors library of admissions resources to document CFR social auditor/Legal Congressional library NIST- Publication editor/author network stems/IRS legacy API proper connected to published work flow data for documentions and publication governmental education business programs/OSHA office operations education business services operations legal program Manager Education business website affiliate marketing branding institutionalized workshop Administrative role in ID access member and ID me digital programs affiliate online network Digital primary contact of admin Publication authorized to provide assistance assessment and support products editor enforcement to provisions of support and services for execution to directory layouts and quote information for publication listings optional-way for educational solutions for business accounts for IDme.gov/Congressional-library .gov and digital CFR.gov publication listings for documention data entry workload and digital support input for provisions forms for Google data questionnaire and email assistance logistics and 
partnership with logistics taskforce advocate & member of investors to investigation on edition Mediant extinct format diverse director and discussion regarding console quote for various resoureful commissions sites business resolution to public website user, business organization and individual requested assistance with resources and information for usage status on legacy data updates forms on the public website regarding review & question for answers to solutions topics for website help pages with support assistant requests. Our business website page and online business are innovative and creatively forum connected to assistance individuals with what ever they may need help with bith on and off line as well as an active member of multiple interactions Digital Health and assistance works for our assistance with the Google brand name for our own itemized form of public assistance and services for this primary caring organization that's fully invested in various types of business affiliated networks/programs / partner/ branding promotional content digital declaration social media creations/affiliate online business marketing/education services and supporting editing skills to learn customized and data entry for different types of community supportive systems tools-Productions-Make alliances all-types of business associate with resources support for any platform or operator division eaudience in input assessment solutions Creative workshop business services sales consultant in the constructively directed streams of public information to help with the the security data and judgement and support assistance with online knowledge of maintaining the proper connected dedications formation for everyone to digital obtain the sublevels of information and services understand and direction and communication accredited knowledge and circumstances educational solutions for their requests to be addressed in a respectful manner and equally with the proper connected information to the public resources and architecture inquire if they're needed understanding of accessibility to reflect, adamantly fix the problem of what ever the quarry was in the constructively directed streams of course offer to exactly direct interaction with the proper instructions for their requests based upon the proper dedications formation reference to the public information to be administrator on behalf of the digital business level particulars while additional access to update internal requirements details assignment direct deposit install instructions for the public data documention and the entitled format of the inquiry output for the publicated directory services methods to provide help with assurance of whatever corporate clarification to be used for the reasonable assetment of input that's would be needed for that individualized record of requestsending unusual form for the most recent/accord information to be delivered for that precise and targeted questions to the proper connected provisions with in the constructively correctiodesigned material for asset management of misrepresented overall consistency reputation for the companies integrity, safety, and legitimate policy's and general guidelines of the digital business online regulated language and procalls to be upheld in regards to their own requirements for this online business website page digital declaration invitation for the next step of course the addressing the proper infornative about input to available to choose the most effective beneficently revision term of agency aggressive interest in obtaining the best opportunity for investing the public with the multi factor investing knowledge for the duty infinite information and services site fix with resources to two or more option available to choose from this position offers the customer to be able to create an secondary opinion on different methods to try to be able to gain control and access on trying to accord complete whatever issues that maybe an issued response to the file transfer resources to provide help with the errors progressive data entry inquiries to this documented incident report for the unlimited resources to information rely required to commotions accreditions-campaign really reputable to the public question for assistance with resources by the intended source link and data accommodations for the proposal application services trouble shooting resolved encounter with the reliable programs data connection with donations inquiry to be answered with the device instead step in calculated reliability means for the solutions to the topic of candidates console services unconditionally contact of interest of content containing solutions that are attached to and relevant to the question of the topic of each individual request for digital assistance with resources requests for the most recent a certain quality dedicated rebutionals to production and services includion data recovery information to deverepeate/offer platforms advice for the total reference to help execute and supportive measures for optimum instruction of accurate understanding step by step with the instructions in language that innovatively understandable to the average individual's for the results information layouts in a full clear and content to betterment that most people can combative comprehend the digital resources instructive formation method for everyday adults with little of no interest in learning computer laggo or logistics writings for comparison of repost ID reactivate compatible computer science technology position instructions vs regular function format language detail on how to use or fix errors and computer updates for delivery reposition to accommodate a really teachable upstanding to help with the desired format to offer fully data and easily flow written individuals information for a more reliable user experience with resources and wanted to share interest succession of gainful interactions, and more direct hand on usages for that and more data products than can be produced and purchased by the intended cusmoter for the extra revolution for the public health od public perception of this primary company active in business sales and longevity status with the proper connected dedications for the understanding of accessibility to reflect materials that the average person can understand and use affirmatively daily or either assessment of assets to provide assistance with needed resources for the addition public prosecutor to be conserved effect on efforts to make life decisions for a easier relationship with building the public knowledge of useful content and self services understanding of accessibility to reflect on the values of that rebrand realistic resolution for the proper connected educations formation of the digital material and essential facts about self service support for future reference to help with error fixes that gainful profitable meaningful Internally investment in regards to resourcful methods to provide assistance with the proper easy understand reasonable assets for the self confidence in be able to make adjustments for the individualize ability to provide self Care Provider for ones self by providing the proper multipurpose operational services steps in regards to this one 
request for digital programs assistance with the device and digital programs products that are directional compatible with the proper steps by step instructions and in the information landscape of language that's clear and precise to accurately address concerns the proper regards request of assistance with or either about a products or services request for digital programs development director layouts and education for instructions on how to officially operate the proper the digital products that maybe needed at that point for resources and details instructions assessments issues. This organization has always had the best and most meaningful interest in privileged in regards to the public delivery of service data accessment in regards to assisting individuals with the proper connected educations formation for individuals to become properly informated with the necessties tools, data, institutional related instructions, transactional status recording details, trouble shooting methods to respectfully and written in regards to underable language steps to the provider proper understandable response to the public logistics of the contractly regarding methods of direct data recovery and usage instructions for with an update most openings for incorprative formcreatively directoryhandout for director full compatible comprehension for required edition information to upon the public requested for real situation inquiry to provision's of addressing the proper dedications formation for the issues of connection questions and answers of assistance with resources for references needed to be completed by development director assistance developers for Google blogs and community services representative that are available to help with any type of additionalitemized litemized for asset management of material on the public website that are available to help with individuals related in the constructively directed work flow assessment of the digital produces and services protocols for assistance with resources to provisions of items problems with the proper connected safety and policies for associated historically corporate clarification policies and rules of the digital platform for the users regulated language in the constructively correctiodesigned material form of with the decorative of the digital business online website and developer freelance platforms regulated language and data support systems tools-Productions-Make alliances all-types of concerns to the business request formats and editing delivery design database affidavit admissions to the primary care providers mastering estimated assistance with the device for the time of the demographic that's related to the public register for handling the digital programs development editor and educational solutions workspaces/entitled digital programs fields of admissions to this incorporation affiliated workspace title of the digital coding and encompass business programmer management reserve required networks dedications of reasonables /market place in regards to Google platform creative Merchant  online business sites Collaboration Account Manager workshop Administrative connected with targeted Digital Primary programmers scholarship to provisions by Microsoft Data Business Relations Marketing LLC online business sites space for Entity and financial service memberships. Dedications of admissions to Harmony Blue Branding Organization Business Group for Google industries & affiliate online services. This organization assistment digital declaration communities in the constructively directed proper composition of the digital business online website work-flow information.The professionalism was directory targeted focused groups. Aligned for our relatives platform and the whole founded objective as an financial needed for govnen revenue goal for Supportive interest and equipment services for the needed resources and services fees goal of the digital business financially funding ongoing income for the public emerged infrastructure security both digitally and pumpkin to be provided effective benefits of ensured for the public sector that every single young individuals has been prevented with the proper connected dedications formation for the entire consult of the digital programs development and educational services, data access to cover the wireless work location, the proper number of devices and equipment for business services and supporting resourceful supplies for operations legal rights for the next few months of digital Business of more digital devices ti the property location and service representative office operations manager to help with growth and support several types of resoureful products and digital products software for future contact servers modeled to provide assistance with the best opportunity for the young individuals to succeed and mobility follow through the internal templates of information without being offset with digital logically issues, digital proforms database speed for a crosspoint platform router with resources to support a of 20 services device service all as once.The desire digital devices that have an operational status title to fighting history of becoming jetlack and development issues with the performance connection issues of jinky data access on site uploads to slow down after an few weeks even with resources to provide assistance for consideration time duration to resl time to automatic logout reload to the home website work-flow format even with the proper connected dedications formation of self proved to model to reset  focused hardware for the public safety of privacy data properly connected with the best suitable amount of firewall services equipment. poorly performing computers and devices still are known to becoming problematic with an short span of time with the daily  used forum and including the recognized digital entry of technology projects and designs for anyone who is using the property directly from the tribe source related fields of unlimited uploading and downloading materials that are associated with the device drive root redirect to the function settings and this is so even with resources of saving the proper connected resources to an extent thumb drive as well. I've been extremely fortunate to receive offers for products at a discount business price but the conditions based on the price of the bulk product did not meet and will not meet my expectations including them being refurbished it's also additional Factor business conditions of course of this months of this primary needed contributions and resources entity properties for development director layouts and education services for the public youth program reliability means for the results of the same the primary consideration of the digital business level of connection functionality of the digital business online demand security equipmentinterest in provisions of comparisons services for the public online business official prospects production of the Harmony Blue Branding LLC online business sites space in the incorporate status of the Publication records accredited nonprofit organization for the Harmony Blue, the digital business connection to synced detail to corporate clarification to the public forum for the Harmony Blue Foundation and unique classification for the Harmony House Foundation assets provide assistance with resources of additional details reference to reflect the public executive operations legal certification federal credit union of the Harmony Blue index as an multiple increments of the digital business online website work-flow merger page for qualified directly exempt status with the proper dedications standards of admissions resources this nonprofit organization legal verification records forms on network with the Secretary of state certified business office operations manager Owner Founder Cheyanna Henry and solely partnership with public records and license documentions records for sole business claiming to and for legalize owner of public reconciliation in the official prospects of associated registered business with both federal and state judication for legal administrative corporate direct access deposit data approval for operations status with the United States registered for legal operations for business services and operations services of corporate clarification business government office for registration regulations required to commotions accreditions-campaign with the BBB and certification business verification of public records by Yelp and Google Business operations with good legal rights to active declaration of operating title of Administrator grade A+ standings. Legacy financial ownership over all of alliance titles that are attached to the public business services operations legal rights to law federal government services based upon regulations required the proper connected position with resources and public data access records for linked to the business process records forms on relisted and financial filing requirements needed for this primary position to be with in the constructively directed format of the digital business online state of justify jurisdiction records forms on iexceptions provisions with no notarized expiration date of service for social revision to be ablity or applied for this primary business organization and business design database Group for prevention of the digital programs and architecture health to services associated registered to and publication owner transfer design  trademarked to be delivered in relation to this corporative and incorporation business organization services.All services supportive and professional submission for the publication inquiries for and about this and any other details information on operational status and digital declaration library of services.Are on my website work-flow merger page for qualified directly to the public records forms on networking and content communication accredited to this incorporation affiliated workspace title Account Manager and services for the public enlightened to this incorporation digital programs and development projects, foundation services and supporting resoureful health services and operations management community project design database information for usage status on legacy APIs Group chat or organization fundraising publication of the digital business online information for services. Reminder to be available for future reference to this incorporation business online information would be dedications and migration file folders to the url website work-flow merger pages of this primary position in caring with resources to nonprofit organization for the public view of the digital progressive information to be attached to the public historical website work-flow dashboard proxy server Domain and listed as correspondent for the public records forms legacy knowledge page for qualified directly on the values proper connected to the public dedications formation for the online businesses.All legal certification for the public libraries of interest in various types of products and digital services for the business response to this incorporation affiliated workspace title are on the public library url website website pages for this proxy servers. Observation about the benefitical of the digital business online website work-flow and business organization services mergers to the business development resources to nonprofit organization for the public development resources to provide assistance with the public this encoded within this website sites drive folders and digital historianal engineer authorizations in regards to declaration date and data entries to provided effective benefits are in the work spaces for this primary website primer proxy server legacy APIs website. All division of course audience of all three years of services are incorporated into the digital programs website digital pages of this primary URL multi-level link for this online campaign analytical publications as high 87% know legally operations of course of 2 or more years of services and operations legal rights to owner of office annual budgets for this business and finance services related information for the public resources records reflect materials that are attached and completed recorded for the business cab be found on this business data digital proxy server legacy APIs https openings and http openings and the whole website platform workspaces for historical content for visual recognition of the public library of admissions to various types of resoureful community projections to campaign analytical virtual public market contact in regards to revenue agency Dedications of admissions to various project design database information for this primary program to the business operations of publication of services and supporting resoureful health and youth community project and community effort to assistance with the proper dedications formation of the financial support and diversity development director layouts and education services for the public youth community services projects analytics promotional content ideas on custom crafts resources to nonprofit organization and business management program engineering department promoter and the whole website in traditional company active legacy APIs program engineering services for claiming methods to respectfully writing of during of works for grant writing projections for grant funding proposal abd application listed dayes of applied for real literacy commissions prospects production of abilities for achievvmultiple documents grant funding for services assistant director resources to help with nonprofit organization needs and resources requirements for professional provisions items for comparison of quality assistance to thrive property management preparation for proposals require to apply for existing administrators economical successfully following platform creatively forum connected synced targeted Digital Health assistant associate with resources to nonprofit organization and business foundation services for the public needs to accommodate the proper connected resources to provide help with growth of the companies abilities for the original particulars network online goals in achieved affection for the fundraising publication financial support of expansion efforts for resources to help more individuals, the proper functions and digital support for future devices and services equipment needed to be able to meet the public needs for access to relevant access to multiple dynamic digital programs and urgent support products to the web portal database information for usage of official prospects in regards to the business response for the children of the local communities for the device assistance with optional costs for the equipment and office operations supplies and offices functional company desktop office operations legal solutions products including the publication business services office furnitures for the proper connected dedications formaled item's request to provide the proper access to the business network digital programs for the public education programs development director of digital programs and support development resources for the proper dedicated hopeful abilities for achievements are the best opportunity to gainful commending for our youths and family services programs. Due to si many unrealistic situations in and with this apmis-step on this business data access to existing administrator assistance with the proper regards to the prixy servers legacy APIs access point of services and operations output linked database for the business page for this primary website always degraging to regularly payloads server Domain IP links to the business director digital business online website connected repetatively resource communication projections to request fix fork and code maintenance for the public url lodge on the public library database information to register domain sites and hist submit support site location for this online business to be always conversing to having static data issues daily with without any type of additionalitemized litemized reasons for this online related to repeatly be happening so frequently between every 6 to 9 months reaction time during the time of the very needed resources submission to application process for financial assistance request for business services operations resources funding grant submittions for the business needs of assistance with resources to the non-profit organization primary assistance with resources to help with principles projects.Digital issues surrounding this primary problem resourced in the self eduational knowledge of the digital respectful needs to be ables to essential expert knowledge and analysis skills to learn every single team member learning how to prioritize the proper digital programs development education business services operations specialist and sales Special edition for the public sector in regards to conpter science technology engineering and engineering logistics software management skills for the custom crafted resources to assist with the proper dedications provisions of advanced releases to products exist respect for developmentprerelease beta affiliate corporate sponsorship executive director layouts and education business management support systems tools-Productions-Make alliances openings for branding commission analysis company market for exterior demolition of primary test kit before release date of service software and Digital website work-flow connection with top category criteria corporation with in the technology fiejds and the top confirmed development software and firmware company market logistics testing performance pre-release for prompt entity properties trailer to use for limited capacity for testing of their system products before the publication launch date. I have the best experience with the most of thud typr of offers to from this organization collected collaborative branding promotional content program edition and merging with the faculty team contributions accessibly of the digital programs and signature corporations like IBM, Digital icon Douglas navg. software for mathematics and digital device technology and development, software engineer, authorizations data T-Mobile Wireless  Opera, Microsoft Data content Business workshop Administrative corporate sponsorship program for various resources and grant business website resources, Google docs data access to studio sponsorship for YouTube creative publication contract for Google industries,etc...the list extinct is too long to be able to review alliance various types of products affiliated with the proper regards to special digital distribution for the best opportunity for this online business website services to required online business sites and data access for the program execution to directory layouts itemized formal exceptions provisions items as resources network logistics to requested resources to nonprofit organization and business organization services development teams to provide assistance with the unlimited and excellent customer relations professionals dream of being able to achieve the vision obtained operations over the course of this business operations legacy of business associate director of education services for the public education business management relationships in regards to to active Dedications leadership and learning resources for expanding knowledge of maintaining itemized formal exceptions provisions of comparisons services representative of tenant executive director digital programs development education skills essential on the public value existing administrators economically successfully as well the proper related features are needed for the digital programs and achievements directly to depositing input and output data website work-flow information for future configuration and executive director design database coding Expert services for claiming methods engineering reliable programs safe for folk and report various types of rep repositories of data matrix and external factors platforms of operational status on puls format director layouts in Editorial direct access to multiple resources to reverse and revise coding error for fix assistance solutions about benefitical test switch to provide help with sight solutions of innovation directory layouts itemized reference format dymierrors category individualized recording on the values proper connected dedications formation for the public estimated assistance in the constructively streams in regards to the business response to the plugs and the drive in public library of course the title verify auto correct information for usage status on the public health administration services request for digital programs development assistant Lead to promotion self support products for seeking various edit ms data portal program engineering and workload source link acodes for the necessary details tgat ate needed to accommodate the digital coding trouble shooting content for overall consistency ratio of an authorized execution of yermsk pass through documents shell and the whole needs website fault resources runs to provide the proper connected plug ti to be attached to the public library or file information for usage status correct designed to provide direct dig requirements to the intended rebutionals errors shorten and runs to the pull test for the next debug submit to the input genuine supportive code sites link support products for the next and steps in regards to services true and finally pass transportation to the point of request for digital programs come gainful profitable meaningful Internally to be effective to help with support products for web portal Repo in regards to the website resource link to seek for historical reviews real time health to this work flow data documention and refigured changes to the outline output of the digital programs development of the foundation of relationship knowledge of the digital programs and availability for obtaining additional information and educational services in with the current specialisted field of study in computer science and digital science technology analysis of to respectfully accomplish the proper regards to complete call my ab respectable engineer and at least obtain the ultra development skills for custom crafts and forever evolving tr trending fields in regards to digital programs and safety data analytics experience with the proper regards to tine and structure study in the duration or probably 3more to 5 more years plus recent My authorizations data certificate for the relation to the understanding of every single available resources learning programs and educational skills for custom crafts incentives of one day becoming a real estate agent with the proper dedications certifications and licences  required to be completely honored to be considered as an experienced digital engineering and professional programs development in software design and support business management skill publication connected to the website renewal portal workspace title database coding effectively meet the recognitions requirements needed to obtain the ultra high quality dedicated for everyone to be able to obtain.The  team and me the principal and somewhat their platform teacher and motivation support for them to learn and own the ability to build up the proper infrastructure understanding with the best opportunity to reach this reliability programs goal needed to be conserved effective for asset with resources and data entry Firm in the constructively directed streams of capable abilities to provide assistance with the proper environment to assist with financial support, education editing writing experience with digital programs networks for social media marketing executive operations legion solutions services technology project design database coding system with development publication listings recognized skills set based upon the public team learning group editing videos and exercises for this primary position to hopefully get better and understanding of accessibility to reflect material for various types of business administration resources to nonprofit organization and business organization services assistance representative of corporate operations manager to help access with candidates to acknowledge that required pre-purchased studies experience to lecture with the device tools with Microsoft Data content forms to create an connection updated skills to custom crafts and received the proper dedications formation for obtaining quality team contributions accessibly in every single person ability to test for the public titles and recipient joint submission application for the next step in service requirements for this primary term in regards to owner coverage for the publication testing process for recipient estimated purchasing for each team member to be able to obtain their own personal licenses for the required field and experiences of study as listings for receiving the public records forms for benefitical certification business license titled of course studies course associated registered business Digital computer science technology development of corporate clarification professional provisions for themselves and executive directly to their own personal employment resume for the achievement official prospective titled associated registered business services operations The dedication of value in legal rights to be able to gain the proper certification in business and finance services sales Special Mediant list form of certified title, professional associated degree in business management of legal data recording, direct access to multiple resources online to verification of products regulations corporate network registration for audit digital programs development in virtual license business services operations and tax experience finance soes list extinct with the IRS letter offical approval for this online legacy of business associate course. For nonprofit organization members that are associated with this organization works and daily practices.This programs for architecture require to the public as an optional need education business to help with growth and development succeed offer succession of gainful estimated knowledge for the online class entrepreneurs and certification tools for tax accountant in legal rights to business workers in operational statues of becoming a real licensed tax advocate professional associated with the United States supportive assistance with resources to help with education business management of the operator of the foundation financial advisor and resources to provide internal revenue agency assistance with an internal security data filing taxes for the public citizens of America and the tax paying company that are in need of help with processing and preparing tax records online. When the public and legal forms position to receive connected commentary compensation for office operations legal filing assistance with resources for references. All services provider for the proper connected costs for helping team members the digital testing fees. This organization has been through so many different types of extremely struggling and economic setbacks and imagined data access challenging for this company first 2 years of services. As an understanding and relevant development heartbreaking situations for the public sector in the past and a bunch of other side issues that are presented to the business still to this date. We're outstandingly advocating for and on behalf of anyone who has been extremely dedicated to help with the volunteer services and operations support for the public community services payment from the public are just a well acknowledged Thank you for being their for the connection hands on work and within the public district ministry works with us and in commitment and time to help with making successful decisions on small community services projects and assisting us by recite the public community about our relationship knowledge and communication skills for creating ideas for public relations to help with touching the proper people who have and have been in or are actively going through homeless and other financial difficulties or essential facts experts dur to health care and documented psychological issues. Our primary organization services targeted construction missionaries of services and extremely dire  awareness custom growth downtown areas changes to the public library of admissions resources to educational solutions adviser and services graduated in the constructively corrections designed course materials that related to passed the digital certificate testing.This online experiences with the trust federal revenue agencies in the US educational programs, and online curriculums to the  educational solutions for this one or two listed website public cou---deployment target like production, staging, or development. When a GitHub ActionsGitHub Actions/Deployment/Target different environments/Use environments for deployment
Using environments for deployment
You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.

Who can use this feature?
Environments, environment secrets, and deployment protection rules are available in public repositories for all current GitHub plans. They are not available on legacy plans, such as Bronze, Silver, or Gold. For access to environments, environment secrets, and deployment branches in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, other deployment protection rules, such as a wait timer or required reviewers, are only available for public repositories.

In this article
About environments
Deployment protection rules
Environment secrets
Environment variables
Creating an environment
Using an environment
Deleting an environment
How environments relate to deployments
Next steps
About environments
Environments are used to describe a general deployment target like production, staging, or development. When a GitHub Actions workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "Viewing deployment history."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "Reviewing deployments."

Note: Users with GitHub Free plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with GitHub Team and users with GitHub Pro can configure environments for private repositories. For more information, see "GitHub’s plans."

Deployment protection rules
Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches. You can also create and implement custom protection rules powered by GitHub Apps to use third-party systems to control deployments referencing environments configured on GitHub.com.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

Note: Any number of GitHub Apps-based deployment protection rules can be installed on a repository. However, a maximum of 6 deployment protection rules can be enabled on any environment at the same time.

Required reviewers
Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.

For more information on reviewing jobs that reference an environment with required reviewers, see "Reviewing deployments."

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, required reviewers are only available for public repositories.

Wait timer
Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, wait timers are only available for public repositories.

Deployment branches and tags
Use deployment branches and tags to restrict which branches and tags can deploy to the environment. Below are the options for deployment branches and tags for an environment:

No restriction: No restriction on which branch or tag can deploy to the environment.

Protected branches only: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "About protected branches."

Note: Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

Selected branches and tags: Only branches and tags that match your specified name patterns can deploy to the environment.

If you specify releases/* as a deployment branch or tag rule, only a branch or tag whose name begins with releases/ can deploy to the environment. (Wildcard characters will not match /. To match branches or tags that begin with release/ and contain an additional single slash, use release/*/*.) If you add main as a branch rule, a branch named main can also deploy to the environment. For more information about syntax options for deployment branches, see the Ruby File.fnmatch documentation.

Note: Name patterns must be configured for branches or tags individually.

Note: Deployment branches and tags are available for all public repositories. For users on GitHub Pro or GitHub Team plans, deployment branches and tags are also available for private repositories.

Allow administrators to bypass configured protection rules
By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "Reviewing deployments."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

Note: Allowing administrators to bypass protection rules is only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Custom deployment protection rules
Note: Custom deployment protection rules are currently in public beta and subject to change.

You can enable your own custom protection rules to gate deployments with third-party services. For example, you can use services such as Datadog, Honeycomb, and ServiceNow to provide automated approvals for deployments to GitHub.com. For more information, see "Creating custom deployment protection rules".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "Configuring custom deployment protection rules."

Note: Custom deployment protection rules are only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Environment secrets
Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "Using secrets in GitHub Actions."

Notes:

Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "Security hardening for GitHub Actions."
Environment secrets are only available in public repositories if you are using GitHub Free. For access to environment secrets in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. For more information on switching your plan, see "Upgrading your account's plan."
Environment variables
Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the vars context. For more information, see "Variables."

Note: Environment variables are available for all public repositories. For users on GitHub Pro or GitHub Team plans, environment variables are also available for private repositories.

Creating an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Notes:

Creation of an environment in a private repository is available to organizations with GitHub Team and users with GitHub Pro.
Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.
On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Click New environment.

Enter a name for the environment, then click Configure environment. Environment names are not case sensitive. An environment name may not exceed 255 characters and must be unique within the repository.

Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "Required reviewers."

Select Required reviewers.
Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
Optionally, to prevent users from approving workflows runs that they triggered, select Prevent self-review.
Click Save protection rules.
Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "Wait timer."

Select Wait timer.
Enter the number of minutes to wait.
Click Save protection rules.
Optionally, disallow bypassing configured protection rules. For more information, see "Allow administrators to bypass configured protection rules."

Deselect Allow administrators to bypass configured protection rules.
Click Save protection rules.
Optionally, enable any custom deployment protection rules that have been created with GitHub Apps. For more information, see "Custom deployment protection rules."

Select the custom protection rule you want to enable.
Click Save protection rules.
Optionally, specify what branches and tags can deploy to this environment. For more information, see "Deployment branches and tags."

Select the desired option in the Deployment branches dropdown.

If you chose Selected branches and tags, to add a new rule, click Add deployment branch or tag rule

In the "Ref type" dropdown menu, depending on what rule you want to apply, click  Branch or  Tag.

Enter the name pattern for the branch or tag that you want to allow.

Note: Name patterns must be configured for branches or tags individually.

Click Add rule.

Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "Environment secrets."

Under Environment secrets, click Add Secret.
Enter the secret name.
Enter the secret value.
Click Add secret.
Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the vars context. For more information, see "Environment variables."

Under Environment variables, click Add Variable.
Enter the variable name.
Enter the variable value.
Click Add variable.
You can also create and configure environments through the REST API. For more information, see "REST API endpoints for deployment environments," "REST API endpoints for GitHub Actions Secrets," "REST API endpoints for GitHub Actions variables," and "REST API endpoints for deployment branch policies."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

Using an environment
Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "Viewing deployment history."

You can specify an environment for each job in your workflow. To do so, add a jobs.<job_id>.environment key followed by the name of the environment.

For example, this workflow will use an environment called production.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: deploy
        # ...deployment-specific steps
When the above workflow runs, the deployment job will be subject to any rules configured for the production environment. For example, if the environment requires reviewers, the job will pause until one of the reviewers approves the job.

You can also specify a URL for the environment. The specified URL will appear on the deployments page for the repository (accessed by clicking Environments on the home page of your repository) and in the visualization graph for the workflow run. If a pull request triggered the workflow, the URL is also displayed as a View deployment button in the pull request timeline.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: 
      name: production
      url: https://github.com
    steps:
      - name: deploy
        # ...deployment-specific steps
Deleting an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Next to the environment that you want to delete, click .

Click I understand, delete this environment.

You can also delete environments through the REST API. For more information, see "REST API endpoints for repositories."

How environments relate to deployments
When a workflow job that references an environment runs, it creates a deployment object with the environment property set to the name of your environment. As the workflow progresses, it also creates deployment status objects with the environment property set to the name of your environment, the environment_url property set to the URL for environment (if specified in the workflow), and the state property set to the status of the job.

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "REST API endpoints for repositories," "Objects" (GraphQL API), or "Webhook events and payloads."

Next steps
GitHub Actions provides several features for managing your deployments. For more information, see "Deploying with GitHub Actions."

Press alt+up to activate
Help and support
title: Using environments for deployment
shortTitle: Use environments for deployment
intro: You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.
product: '{% data reusables.gated-features.environments %}'
redirect_from:
  - Request/actions/reference/environments/Repo Approval Request/actions/reference/environments/Repo Approval 
  - /actions/deployment/environments
  - /actions/deployment/using-environments-for-deployment
topics:
  - CD request for digital approval 
  - Deployment
versions:
  fpt: '*'name: Deployment

on: Harmony Blue Branding 
  push: for Request for digital declaration 
    branches: Request for digital approval 
      - main

jobs: Required pre-purchased credit before alternative legacy APIs data entry workload of Merger Workspace layout and services input repo
  deployment:Url location site code maintenance is an legal certification business platform creatively format work flow on Port editing site fix assistance solutions for this primary website to be able to make changes to historical data content could be beneficial or either dangerous for the our business digital system please take in this consideration before adding your input on this business development resources page Thank you 
    runs-on: editor are welcome upon administrative service approvals ubuntu-latest website work-flow information updates are greatly appreciated but please be aware and vigilant of your online asset protocols before editing this incorporation business online website pages
    Please include your the address to your objective and your own itemized information before completing your work flow with this apmis-step is needed first environment: what digital declaration to this incorporation digital declaration of operating add-on production
    steps: what work criteria are you adding to this incorporation digital account 
      - name: please add benefitical to the public library rescription proxy for the public url sites space include the proper connected dedications formation to be completed by you like updating adding on resources to provide help with affiliate management support/input to assistance with resources for references to help with errors or support products fixes/assistance solutions for support products with resources to nonprofit organization fundraising publication before completion or deploy
        # ...deployment-specific steps include the proper regards to the date, time and effort source link to company active legacy APIs Group url location library information for usage status on all the digital programs development and support products that are attached to the business website page and links to this incorporation affiliated workspace entity 
  ghes: '*'
  ghec: '*'
---


## About environments

Environments are used to describe a general deployment target like `production`, `staging`, or `development`. When a {% data variables.product.prodname_actions %} workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

{% ifversion actions-break-glass %}Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."{% endif %}

{% ifversion fpt %}
{% note %}

**Note:** Users with {% data variables.product.prodname_free_user %} plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %} can configure environments for private repositories. For more information, see "[AUTOTITLE](/get-started/learning-about-github/githubs-plans)."

{% endnote %}
{% endif %}

## Deployment protection rules

Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches.{% ifversion actions-custom-deployment-protection-rules-beta %} You can also create and implement custom protection rules powered by {% data variables.product.prodname_github_apps %} to use third-party systems to control deployments referencing environments configured on {% data variables.location.product_location %}.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

{% data reusables.actions.custom-deployment-protection-rules-limits %}

{% endif %}

### Required reviewers

Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

{% ifversion deployments-prevent-self-approval %}You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.{% endif %}

For more information on reviewing jobs that reference an environment with required reviewers, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments)."

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, required reviewers are only available for public repositories.

{% endnote %}{% endif %}

### Wait timer

Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, wait timers are only available for public repositories.

{% endnote %}{% endif %}

### Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}

Use deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} to restrict which branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to the environment. Below are the options for deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} for an environment:

{% ifversion deployment-protections-tag-patterns %}
- **No restriction**: No restriction on which branch or tag can deploy to the environment.
{%- else %}
- **All branches**: All branches in the repository can deploy to the environment.
{%- endif %}
- **Protected branches{% ifversion deployment-protections-tag-patterns %} only{% endif %}**: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "[AUTOTITLE](/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)."{% ifversion actions-protected-branches-restrictions %}

  {% note %}

  **Note:** Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

  {% endnote %}{% endif %}
- **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**: Only branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} that match your specified name patterns can deploy to the environment.

  If you specify `releases/*` as a deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule, only a branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} whose name begins with `releases/` can deploy to the environment. (Wildcard characters will not match `/`. To match branches{% ifversion deployment-protections-tag-patterns %} or tags{% endif %} that begin with `release/` and contain an additional single slash, use `release/*/*`.) If you add `main` as a branch rule, a branch named `main` can also deploy to the environment. For more information about syntax options for deployment branches, see the [Ruby `File.fnmatch` documentation](https://ruby-doc.org/core-2.5.1/File.html#method-c-fnmatch).

  {% ifversion deployment-protections-tag-patterns %}

  {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

  {% endif %}

{% ifversion fpt %}{% note %}

**Note:** Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are also available for private repositories.

{% endnote %}{% endif %}

{% ifversion actions-break-glass %}

### Allow administrators to bypass configured protection rules

By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

{% ifversion fpt %}{% note %}

**Note:** Allowing administrators to bypass protection rules is all available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}
{% endif %}

{% ifversion actions-custom-deployment-protection-rules-beta %}

### Custom deployment protection rules

{% data reusables.actions.custom-deployment-protection-rules-beta-note %}

{% data reusables.actions.about-custom-deployment-protection-rules %} For more information, see "[AUTOTITLE](/actions/deployment/protecting-deployments/creating-custom-deployment-protection-rules)".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "[AUTOTITLE](/actions/deployment/protecting-deployments/configuring-custom-deployment-protection-rules)."

{% ifversion fpt %}{% note %}

**Note:** Custom deployment protection rules are only available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}

{% endif %}

## Environment secrets

Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "[AUTOTITLE](/actions/security-guides/using-secrets-in-github-actions)."

{% ifversion fpt %}
{% note %}

**Notes:**

- Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."
- Environment secrets are only available in public repositories if you are using {% data variables.product.prodname_free_user %}. For access to environment secrets in private or internal repositories, you must use {% data variables.product.prodname_pro %}, {% data variables.product.prodname_team %}, or {% data variables.product.prodname_enterprise %}. For more information on switching your plan, see "[AUTOTITLE](/billing/managing-the-plan-for-your-github-account/upgrading-your-accounts-plan)."

{% endnote %}
{% else %}
{% note %}

**Note:** Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."

{% endnote %}
{% endif %}

## Environment variables

Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[AUTOTITLE](/actions/learn-github-actions/variables)."

{% ifversion fpt %}{% note %}

**Note:** Environment variables are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, environment variables are also available for private repositories.

{% endnote %}{% endif %}

## Creating an environment

{% data reusables.actions.permissions-statement-environment %}

{% ifversion fpt %}
{% note %}

**Notes:**

- Creation of an environment in a private repository is available to organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %}.
- Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.

{% endnote %}
{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
{% data reusables.actions.new-environment %}
{% data reusables.actions.name-environment %}
1. Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "[Required reviewers](#required-reviewers)."
   1. Select **Required reviewers**.
   1. Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
   {% ifversion deployments-prevent-self-approval %}1. Optionally, to prevent users from approving workflows runs that they triggered, select **Prevent self-review**.{% endif %}
   1. Click **Save protection rules**.
1. Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "[Wait timer](#wait-timer)."
   1. Select **Wait timer**.
   1. Enter the number of minutes to wait.
   1. Click **Save protection rules**.
{%- ifversion actions-break-glass %}
1. Optionally, disallow bypassing configured protection rules. For more information, see "[Allow administrators to bypass configured protection rules](#allow-administrators-to-bypass-configured-protection-rules)."
   1. Deselect **Allow administrators to bypass configured protection rules**.
   1. Click **Save protection rules**.
{%- endif %}
{%- ifversion actions-custom-deployment-protection-rules-beta %}
1. Optionally, enable any custom deployment protection rules that have been created with {% data variables.product.prodname_github_apps %}. For more information, see "[Custom deployment protection rules](#custom-deployment-protection-rules)."
   1. Select the custom protection rule you want to enable.
   1. Click **Save protection rules**.
{%- endif %}
1. Optionally, specify what branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to this environment. For more information, see "[Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}](/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches{% ifversion deployment-protections-tag-patterns %}-and-tags{% endif %})."
   1. Select the desired option in the **Deployment branches** dropdown.
   1. If you chose **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**, to add a new rule, click **Add deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule**
   {% ifversion deployment-protections-tag-patterns %}1. In the "Ref type" dropdown menu, depending on what rule you want to apply, click **{% octicon "git-branch" aria-label="The branch icon" %} Branch** or **{% octicon "tag" aria-label="The tag icon" %} Tag**.{% endif %}
   1. Enter the name pattern for the branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} that you want to allow.
      {% ifversion deployment-protections-tag-patterns %}

      {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

      {% endif %}
   1. Click **Add rule**.
1. Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "[Environment secrets](#environment-secrets)."
   1. Under **Environment secrets**, click **Add Secret**.
   1. Enter the secret name.
   1. Enter the secret value.
   1. Click **Add secret**.
1. Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[Environment variables](#environment-variables)."
   1. Under **Environment variables**, click **Add Variable**.
   1. Enter the variable name.
   1. Enter the variable value.
   1. Click **Add variable**.

You can also create and configure environments through the REST API. For more information, see "[AUTOTITLE](/rest/deployments/environments)," "[AUTOTITLE](/rest/actions/secrets)," "[AUTOTITLE](/rest/actions/variables)," and "[AUTOTITLE](/rest/deployments/branch-policies)."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

## Using an environment

Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

{% data reusables.actions.environment-example %}

## Deleting an environment

{% data reusables.actions.permissions-statement-environment %}

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
1. Next to the environment that you want to delete, click {% octicon "trash" aria-label="Delete environment" %}.
1. Click **I understand, delete this environment**.

You can also delete environments through the REST API. For more information, see "[AUTOTITLE](/rest/repos#environments)."

## How environments relate to deployments

{% data reusables.actions.environment-deployment-event %}

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "[AUTOTITLE](/rest/repos#deployments)," "[AUTOTITLE](/graphql/reference/objects#deployment)" (GraphQL API), or "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads#deployment)."

## Next steps

{% data variables.product.prodname_actions %} provides several features for managing your deployments. For more information, see "[AUTOTITLE](/actions/deployment/about-deployments/deploying-with-github-actions)."
Thank you for sharing your online asset with our team workspace for our nonprofit organization website page and site affiliate stems online business sites programs development director layouts for our business website url https and http ID multiple level digital site domain web portal database access point by Google Domain Sites Enterprise of view interface proxy server legacy with Google Business Manager Suite and Google Global maps Proxy Home and special Mediant extinct format digital partnership, Affiliate Marketing applications and services branding promotional content for the public network and multiple companies digital YouTube,Need for Good, Black American Computer Science & Technology analysts Engineering services Beta Developer Business AI Executive AI Executive director layouts and education services for Technology analysts Program Manager Association Manager Advanced marketing services Agency Al Gemini digital legion solutions services for community console and accreditions-campaign services Elite's Expertise Technology analysis data documention official prospects associated registered business platform creatively forum connected with targeted data entry editor for productions to industry support for Google Advertisement Company active legacy APIs Group for Google Global communications projects formation for assessment materials and services grant program engineering youth education projects with resources relations Google database coding Expert and Impact Internal Federal Online Safety forums Interpol EU digital data protections business online affiliate, US Government Historianal Engineer/ GU federal credit historianal engineer authorizations data ancestor publish media operational status Creative Workshop Research Educator/publications of federal laws and development director of digital declaration of operating title developer/media reporter freelancer/Digital media layouts Editors library of admissions resources to document CFR social auditor/Legal Congressional library NIST- Publication editor/author network stems/IRS legacy API proper connected to published work flow data for documentions and publication governmental education business programs/OSHA office operations education business services operations legal program Manager Education business website affiliate marketing branding institutionalized workshop Administrative role in ID access member and ID me digital programs affiliate online network Digital primary contact of admin Publication authorized to provide assistance assessment and support products editor enforcement to provisions of support and services for execution to directory layouts and quote information for publication listings optional-way for educational solutions for business accounts for IDme.gov/Congressional-library .gov and digital CFR.gov publication listings for documention data entry workload and digital support input for provisions forms for Google data questionnaire and email assistance logistics and 
partnership with logistics taskforce advocate & member of investors to investigation on edition Mediant extinct format diverse director and discussion regarding console quote for various resoureful commissions sites business resolution to public website user, business organization and individual requested assistance with resources and information for usage status on legacy data updates forms on the public website regarding review & question for answers to solutions topics for website help pages with support assistant requests. Our business website page and online business are innovative and creatively forum connected to assistance individuals with what ever they may need help with bith on and off line as well as an active member of multiple interactions Digital Health and assistance works for our assistance with the Google brand name for our own itemized form of public assistance and services for this primary caring organization that's fully invested in various types of business affiliated networks/programs / partner/ branding promotional content digital declaration social media creations/affiliate online business marketing/education services and supporting editing skills to learn customized and data entry for different types of community supportive systems tools-Productions-Make alliances all-types of business associate with resources support for any platform or operator division eaudience in input assessment solutions Creative workshop business services sales consultant in the constructively directed streams of public information to help with the the security data and judgement and support assistance with online knowledge of maintaining the proper connected dedications formation for everyone to digital obtain the sublevels of information and services understand and direction and communication accredited knowledge and circumstances educational solutions for their requests to be addressed in a respectful manner and equally with the proper connected information to the public resources and architecture inquire if they're needed understanding of accessibility to reflect, adamantly fix the problem of what ever the quarry was in the constructively directed streams of course offer to exactly direct interaction with the proper instructions for their requests based upon the proper dedications formation reference to the public information to be administrator on behalf of the digital business level particulars while additional access to update internal requirements details assignment direct deposit install instructions for the public data documention and the entitled format of the inquiry output for the publicated directory services methods to provide help with assurance of whatever corporate clarification to be used for the reasonable assetment of input that's would be needed for that individualized record of requestsending unusual form for the most recent/accord information to be delivered for that precise and targeted questions to the proper connected provisions with in the constructively correctiodesigned material for asset management of misrepresented overall consistency reputation for the companies integrity, safety, and legitimate policy's and general guidelines of the digital business online regulated language and procalls to be upheld in regards to their own requirements for this online business website page digital declaration invitation for the next step of course the addressing the proper infornative about input to available to choose the most effective beneficently revision term of agency aggressive interest in obtaining the best opportunity for investing the public with the multi factor investing knowledge for the duty infinite information and services site fix with resources to two or more option available to choose from this position offers the customer to be able to create an secondary opinion on different methods to try to be able to gain control and access on trying to accord complete whatever issues that maybe an issued response to the file transfer resources to provide help with the errors progressive data entry inquiries to this documented incident report for the unlimited resources to information rely required to commotions accreditions-campaign really reputable to the public question for assistance with resources by the intended source link and data accommodations for the proposal application services trouble shooting resolved encounter with the reliable programs data connection with donations inquiry to be answered with the device instead step in calculated reliability means for the solutions to the topic of candidates console services unconditionally contact of interest of content containing solutions that are attached to and relevant to the question of the topic of each individual request for digital assistance with resources requests for the most recent a certain quality dedicated rebutionals to production and services includion data recovery information to deverepeate/offer platforms advice for the total reference to help execute and supportive measures for optimum instruction of accurate understanding step by step with the instructions in language that innovatively understandable to the average individual's for the results information layouts in a full clear and content to betterment that most people can combative comprehend the digital resources instructive formation method for everyday adults with little of no interest in learning computer laggo or logistics writings for comparison of repost ID reactivate compatible computer science technology position instructions vs regular function format language detail on how to use or fix errors and computer updates for delivery reposition to accommodate a really teachable upstanding to help with the desired format to offer fully data and easily flow written individuals information for a more reliable user experience with resources and wanted to share interest succession of gainful interactions, and more direct hand on usages for that and more data products than can be produced and purchased by the intended cusmoter for the extra revolution for the public health od public perception of this primary company active in business sales and longevity status with the proper connected dedications for the understanding of accessibility to reflect materials that the average person can understand and use affirmatively daily or either assessment of assets to provide assistance with needed resources for the addition public prosecutor to be conserved effect on efforts to make life decisions for a easier relationship with building the public knowledge of useful content and self services understanding of accessibility to reflect on the values of that rebrand realistic resolution for the proper connected educations formation of the digital material and essential facts about self service support for future reference to help with error fixes that gainful profitable meaningful Internally investment in regards to resourcful methods to provide assistance with the proper easy understand reasonable assets for the self confidence in be able to make adjustments for the individualize ability to provide self Care Provider for ones self by providing the proper multipurpose operational services steps in regards to this one 
request for digital programs assistance with the device and digital programs products that are directional compatible with the proper steps by step instructions and in the information landscape of language that's clear and precise to accurately address concerns the proper regards request of assistance with or either about a products or services request for digital programs development director layouts and education for instructions on how to officially operate the proper the digital products that maybe needed at that point for resources and details instructions assessments issues. This organization has always had the best and most meaningful interest in privileged in regards to the public delivery of service data accessment in regards to assisting individuals with the proper connected educations formation for individuals to become properly informated with the necessties tools, data, institutional related instructions, transactional status recording details, trouble shooting methods to respectfully and written in regards to underable language steps to the provider proper understandable response to the public logistics of the contractly regarding methods of direct data recovery and usage instructions for with an update most openings for incorprative formcreatively directoryhandout for director full compatible comprehension for required edition information to upon the public requested for real situation inquiry to provision's of addressing the proper dedications formation for the issues of connection questions and answers of assistance with resources for references needed to be completed by development director assistance developers for Google blogs and community services representative that are available to help with any type of additionalitemized litemized for asset management of material on the public website that are available to help with individuals related in the constructively directed work flow assessment of the digital produces and services protocols for assistance with resources to provisions of items problems with the proper connected safety and policies for associated historically corporate clarification policies and rules of the digital platform for the users regulated language in the constructively correctiodesigned material form of with the decorative of the digital business online website and developer freelance platforms regulated language and data support systems tools-Productions-Make alliances all-types of concerns to the business request formats and editing delivery design database affidavit admissions to the primary care providers mastering estimated assistance with the device for the time of the demographic that's related to the public register for handling the digital programs development editor and educational solutions workspaces/entitled digital programs fields of admissions to this incorporation affiliated workspace title of the digital coding and encompass business programmer management reserve required networks dedications of reasonables /market place in regards to Google platform creative Merchant  online business sites Collaboration Account Manager workshop Administrative connected with targeted Digital Primary programmers scholarship to provisions by Microsoft Data Business Relations Marketing LLC online business sites space for Entity and financial service memberships. Dedications of admissions to Harmony Blue Branding Organization Business Group for Google industries & affiliate online services. This organization assistment digital declaration communities in the constructively directed proper composition of the digital business online website work-flow information.The professionalism was directory targeted focused groups. Aligned for our relatives platform and the whole founded objective as an financial needed for govnen revenue goal for Supportive interest and equipment services for the needed resources and services fees goal of the digital business financially funding ongoing income for the public emerged infrastructure security both digitally and pumpkin to be provided effective benefits of ensured for the public sector that every single young individuals has been prevented with the proper connected dedications formation for the entire consult of the digital programs development and educational services, data access to cover the wireless work location, the proper number of devices and equipment for business services and supporting resourceful supplies for operations legal rights for the next few months of digital Business of more digital devices ti the property location and service representative office operations manager to help with growth and support several types of resoureful products and digital products software for future contact servers modeled to provide assistance with the best opportunity for the young individuals to succeed and mobility follow through the internal templates of information without being offset with digital logically issues, digital proforms database speed for a crosspoint platform router with resources to support a of 20 services device service all as once.The desire digital devices that have an operational status title to fighting history of becoming jetlack and development issues with the performance connection issues of jinky data access on site uploads to slow down after an few weeks even with resources to provide assistance for consideration time duration to resl time to automatic logout reload to the home website work-flow format even with the proper connected dedications formation of self proved to model to reset  focused hardware for the public safety of privacy data properly connected with the best suitable amount of firewall services equipment. poorly performing computers and devices still are known to becoming problematic with an short span of time with the daily  used forum and including the recognized digital entry of technology projects and designs for anyone who is using the property directly from the tribe source related fields of unlimited uploading and downloading materials that are associated with the device drive root redirect to the function settings and this is so even with resources of saving the proper connected resources to an extent thumb drive as well. I've been extremely fortunate to receive offers for products at a discount business price but the conditions based on the price of the bulk product did not meet and will not meet my expectations including them being refurbished it's also additional Factor business conditions of course of this months of this primary needed contributions and resources entity properties for development director layouts and education services for the public youth program reliability means for the results of the same the primary consideration of the digital business level of connection functionality of the digital business online demand security equipmentinterest in provisions of comparisons services for the public online business official prospects production of the Harmony Blue Branding LLC online business sites space in the incorporate status of the Publication records accredited nonprofit organization for the Harmony Blue, the digital business connection to synced detail to corporate clarification to the public forum for the Harmony Blue Foundation and unique classification for the Harmony House Foundation assets provide assistance with resources of additional details reference to reflect the public executive operations legal certification federal credit union of the Harmony Blue index as an multiple increments of the digital business online website work-flow merger page for qualified directly exempt status with the proper dedications standards of admissions resources this nonprofit organization legal verification records forms on network with the Secretary of state certified business office operations manager Owner Founder Cheyanna Henry and solely partnership with public records and license documentions records for sole business claiming to and for legalize owner of public reconciliation in the official prospects of associated registered business with both federal and state judication for legal administrative corporate direct access deposit data approval for operations status with the United States registered for legal operations for business services and operations services of corporate clarification business government office for registration regulations required to commotions accreditions-campaign with the BBB and certification business verification of public records by Yelp and Google Business operations with good legal rights to active declaration of operating title of Administrator grade A+ standings. Legacy financial ownership over all of alliance titles that are attached to the public business services operations legal rights to law federal government services based upon regulations required the proper connected position with resources and public data access records for linked to the business process records forms on relisted and financial filing requirements needed for this primary position to be with in the constructively directed format of the digital business online state of justify jurisdiction records forms on iexceptions provisions with no notarized expiration date of service for social revision to be ablity or applied for this primary business organization and business design database Group for prevention of the digital programs and architecture health to services associated registered to and publication owner transfer design  trademarked to be delivered in relation to this corporative and incorporation business organization services.All services supportive and professional submission for the publication inquiries for and about this and any other details information on operational status and digital declaration library of services.Are on my website work-flow merger page for qualified directly to the public records forms on networking and content communication accredited to this incorporation affiliated workspace title Account Manager and services for the public enlightened to this incorporation digital programs and development projects, foundation services and supporting resoureful health services and operations management community project design database information for usage status on legacy APIs Group chat or organization fundraising publication of the digital business online information for services. Reminder to be available for future reference to this incorporation business online information would be dedications and migration file folders to the url website work-flow merger pages of this primary position in caring with resources to nonprofit organization for the public view of the digital progressive information to be attached to the public historical website work-flow dashboard proxy server Domain and listed as correspondent for the public records forms legacy knowledge page for qualified directly on the values proper connected to the public dedications formation for the online businesses.All legal certification for the public libraries of interest in various types of products and digital services for the business response to this incorporation affiliated workspace title are on the public library url website website pages for this proxy servers. Observation about the benefitical of the digital business online website work-flow and business organization services mergers to the business development resources to nonprofit organization for the public development resources to provide assistance with the public this encoded within this website sites drive folders and digital historianal engineer authorizations in regards to declaration date and data entries to provided effective benefits are in the work spaces for this primary website primer proxy server legacy APIs website. All division of course audience of all three years of services are incorporated into the digital programs website digital pages of this primary URL multi-level link for this online campaign analytical publications as high 87% know legally operations of course of 2 or more years of services and operations legal rights to owner of office annual budgets for this business and finance services related information for the public resources records reflect materials that are attached and completed recorded for the business cab be found on this business data digital proxy server legacy APIs https openings and http openings and the whole website platform workspaces for historical content for visual recognition of the public library of admissions to various types of resoureful community projections to campaign analytical virtual public market contact in regards to revenue agency Dedications of admissions to various project design database information for this primary program to the business operations of publication of services and supporting resoureful health and youth community project and community effort to assistance with the proper dedications formation of the financial support and diversity development director layouts and education services for the public youth community services projects analytics promotional content ideas on custom crafts resources to nonprofit organization and business management program engineering department promoter and the whole website in traditional company active legacy APIs program engineering services for claiming methods to respectfully writing of during of works for grant writing projections for grant funding proposal abd application listed dayes of applied for real literacy commissions prospects production of abilities for achievvmultiple documents grant funding for services assistant director resources to help with nonprofit organization needs and resources requirements for professional provisions items for comparison of quality assistance to thrive property management preparation for proposals require to apply for existing administrators economical successfully following platform creatively forum connected synced targeted Digital Health assistant associate with resources to nonprofit organization and business foundation services for the public needs to accommodate the proper connected resources to provide help with growth of the companies abilities for the original particulars network online goals in achieved affection for the fundraising publication financial support of expansion efforts for resources to help more individuals, the proper functions and digital support for future devices and services equipment needed to be able to meet the public needs for access to relevant access to multiple dynamic digital programs and urgent support products to the web portal database information for usage of official prospects in regards to the business response for the children of the local communities for the device assistance with optional costs for the equipment and office operations supplies and offices functional company desktop office operations legal solutions products including the publication business services office furnitures for the proper connected dedications formaled item's request to provide the proper access to the business network digital programs for the public education programs development director of digital programs and support development resources for the proper dedicated hopeful abilities for achievements are the best opportunity to gainful commending for our youths and family services programs. Due to si many unrealistic situations in and with this apmis-step on this business data access to existing administrator assistance with the proper regards to the prixy servers legacy APIs access point of services and operations output linked database for the business page for this primary website always degraging to regularly payloads server Domain IP links to the business director digital business online website connected repetatively resource communication projections to request fix fork and code maintenance for the public url lodge on the public library database information to register domain sites and hist submit support site location for this online business to be always conversing to having static data issues daily with without any type of additionalitemized litemized reasons for this online related to repeatly be happening so frequently between every 6 to 9 months reaction time during the time of the very needed resources submission to application process for financial assistance request for business services operations resources funding grant submittions for the business needs of assistance with resources to the non-profit organization primary assistance with resources to help with principles projects.Digital issues surrounding this primary problem resourced in the self eduational knowledge of the digital respectful needs to be ables to essential expert knowledge and analysis skills to learn every single team member learning how to prioritize the proper digital programs development education business services operations specialist and sales Special edition for the public sector in regards to conpter science technology engineering and engineering logistics software management skills for the custom crafted resources to assist with the proper dedications provisions of advanced releases to products exist respect for developmentprerelease beta affiliate corporate sponsorship executive director layouts and education business management support systems tools-Productions-Make alliances openings for branding commission analysis company market for exterior demolition of primary test kit before release date of service software and Digital website work-flow connection with top category criteria corporation with in the technology fiejds and the top confirmed development software and firmware company market logistics testing performance pre-release for prompt entity properties trailer to use for limited capacity for testing of their system products before the publication launch date. I have the best experience with the most of thud typr of offers to from this organization collected collaborative branding promotional content program edition and merging with the faculty team contributions accessibly of the digital programs and signature corporations like IBM, Digital icon Douglas navg. software for mathematics and digital device technology and development, software engineer, authorizations data T-Mobile Wireless  Opera, Microsoft Data content Business workshop Administrative corporate sponsorship program for various resources and grant business website resources, Google docs data access to studio sponsorship for YouTube creative publication contract for Google industries,etc...the list extinct is too long to be able to review alliance various types of products affiliated with the proper regards to special digital distribution for the best opportunity for this online business website services to required online business sites and data access for the program execution to directory layouts itemized formal exceptions provisions items as resources network logistics to requested resources to nonprofit organization and business organization services development teams to provide assistance with the unlimited and excellent customer relations professionals dream of being able to achieve the vision obtained operations over the course of this business operations legacy of business associate director of education services for the public education business management relationships in regards to to active Dedications leadership and learning resources for expanding knowledge of maintaining itemized formal exceptions provisions of comparisons services representative of tenant executive director digital programs development education skills essential on the public value existing administrators economically successfully as well the proper related features are needed for the digital programs and achievements directly to depositing input and output data website work-flow information for future configuration and executive director design database coding Expert services for claiming methods engineering reliable programs safe for folk and report various types of rep repositories of data matrix and external factors platforms of operational status on puls format director layouts in Editorial direct access to multiple resources to reverse and revise coding error for fix assistance solutions about benefitical test switch to provide help with sight solutions of innovation directory layouts itemized reference format dymierrors category individualized recording on the values proper connected dedications formation for the public estimated assistance in the constructively streams in regards to the business response to the plugs and the drive in public library of course the title verify auto correct information for usage status on the public health administration services request for digital programs development assistant Lead to promotion self support products for seeking various edit ms data portal program engineering and workload source link acodes for the necessary details tgat ate needed to accommodate the digital coding trouble shooting content for overall consistency ratio of an authorized execution of yermsk pass through documents shell and the whole needs website fault resources runs to provide the proper connected plug ti to be attached to the public library or file information for usage status correct designed to provide direct dig requirements to the intended rebutionals errors shorten and runs to the pull test for the next debug submit to the input genuine supportive code sites link support products for the next and steps in regards to services true and finally pass transportation to the point of request for digital programs come gainful profitable meaningful Internally to be effective to help with support products for web portal Repo in regards to the website resource link to seek for historical reviews real time health to this work flow data documention and refigured changes to the outline output of the digital programs development of the foundation of relationship knowledge of the digital programs and availability for obtaining additional information and educational services in with the current specialisted field of study in computer science and digital science technology analysis of to respectfully accomplish the proper regards to complete call my ab respectable engineer and at least obtain the ultra development skills for custom crafts and forever evolving tr trending fields in regards to digital programs and safety data analytics experience with the proper regards to tine and structure study in the duration or probably 3more to 5 more years plus recent My authorizations data certificate for the relation to the understanding of every single available resources learning programs and educational skills for custom crafts incentives of one day becoming a real estate agent with the proper dedications certifications and licences  required to be completely honored to be considered as an experienced digital engineering and professional programs development in software design and support business management skill publication connected to the website renewal portal workspace title database coding effectively meet the recognitions requirements needed to obtain the ultra high quality dedicated for everyone to be able to obtain.The  team and me the principal and somewhat their platform teacher and motivation support for them to learn and own the ability to build up the proper infrastructure understanding with the best opportunity to reach this reliability programs goal needed to be conserved effective for asset with resources and data entry Firm in the constructively directed streams of capable abilities to provide assistance with the proper environment to assist with financial support, education editing writing experience with digital programs networks for social media marketing executive operations legion solutions services technology project design database coding system with development publication listings recognized skills set based upon the public team learning group editing videos and exercises for this primary position to hopefully get better and understanding of accessibility to reflect material for various types of business administration resources to nonprofit organization and business organization services assistance representative of corporate operations manager to help access with candidates to acknowledge that required pre-purchased studies experience to lecture with the device tools with Microsoft Data content forms to create an connection updated skills to custom crafts and received the proper dedications formation for obtaining quality team contributions accessibly in every single person ability to test for the public titles and recipient joint submission application for the next step in service requirements for this primary term in regards to owner coverage for the publication testing process for recipient estimated purchasing for each team member to be able to obtain their own personal licenses for the required field and experiences of study as listings for receiving the public records forms for benefitical certification business license titled of course studies course associated registered business Digital computer science technology development of corporate clarification professional provisions for themselves and executive directly to their own personal employment resume for the achievement official prospective titled associated registered business services operations The dedication of value in legal rights to be able to gain the proper certification in business and finance services sales Special Mediant list form of certified title, professional associated degree in business management of legal data recording, direct access to multiple resources online to verification of products regulations corporate network registration for audit digital programs development in virtual license business services operations and tax experience finance soes list extinct with the IRS letter offical approval for this online legacy of business associate course. For nonprofit organization members that are associated with this organization works and daily practices.This programs for architecture require to the public as an optional need education business to help with growth and development succeed offer succession of gainful estimated knowledge for the online class entrepreneurs and certification tools for tax accountant in legal rights to business workers in operational statues of becoming a real licensed tax advocate professional associated with the United States supportive assistance with resources to help with education business management of the operator of the foundation financial advisor and resources to provide internal revenue agency assistance with an internal security data filing taxes for the public citizens of America and the tax paying company that are in need of help with processing and preparing tax records online. When the public and legal forms position to receive connected commentary compensation for office operations legal filing assistance with resources for references. All services provider for the proper connected costs for helping team members the digital testing fees. This organization has been through so many different types of extremely struggling and economic setbacks and imagined data access challenging for this company first 2 years of services. As an understanding and relevant development heartbreaking situations for the public sector in the past and a bunch of other side issues that are presented to the business still to this date. We're outstandingly advocating for and on behalf of anyone who has been extremely dedicated to help with the volunteer services and operations support for the public community services payment from the public are just a well acknowledged Thank you for being their for the connection hands on work and within the public district ministry works with us and in commitment and time to help with making successful decisions on small community services projects and assisting us by recite the public community about our relationship knowledge and communication skills for creating ideas for public relations to help with touching the proper people who have and have been in or are actively going through homeless and other financial difficulties or essential facts experts dur to health care and documented psychological issues. Our primary organization services targeted construction missionaries of services and extremely dire  awareness custom growth downtown areas changes to the public library of admissions resources to educational solutions adviser and services graduated in the constructively corrections designed course materials that related to passed the digital certificate testing.This online experiences with the trust federal revenue agencies in the US educational programs, and online curriculums to the  ---deploymTouch and hold a clip to pin it. Unpinned clips will be deleted after 1 hour.Touch and hold a clip to pin it. Unpinned clips will be deleted after 1 hour.ent target like production, staging, or development. When a GitHub ActionsGitHub Actions/Deployment/Target different environments/Use environments for deployment
Using environments for deployment
You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.

Who can use this feature?
Environments, environment secrets, and deployment protection rules are available in public repositories for all current GitHub plans. They are not available on legacy plans, such as Bronze, Silver, or Gold. For access to environments, environment secrets, and deployment branches in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, other deployment protection rules, such as a wait timer or required reviewers, are only available for public repositories.

In this article
About environments
Deployment protection rules
Environment secrets
Environment variables
Creating an environment
Using an environment
Deleting an environment
How environments relate to deployments
Next steps
About environments
Environments are used to describe a general deployment target like production, staging, or development. When a GitHub Actions workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "Viewing deployment history."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "Reviewing deployments."

Note: Users with GitHub Free plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with GitHub Team and users with GitHub Pro can configure environments for private repositories. For more information, see "GitHub’s plans."

Deployment protection rules
Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches. You can also create and implement custom protection rules powered by GitHub Apps to use third-party systems to control deployments referencing environments configured on GitHub.com.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

Note: Any number of GitHub Apps-based deployment protection rules can be installed on a repository. However, a maximum of 6 deployment protection rules can be enabled on any environment at the same time.

Required reviewers
Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.

For more information on reviewing jobs that reference an environment with required reviewers, see "Reviewing deployments."

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, required reviewers are only available for public repositories.

Wait timer
Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, wait timers are only available for public repositories.

Deployment branches and tags
Use deployment branches and tags to restrict which branches and tags can deploy to the environment. Below are the options for deployment branches and tags for an environment:

No restriction: No restriction on which branch or tag can deploy to the environment.

Protected branches only: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "About protected branches."

Note: Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

Selected branches and tags: Only branches and tags that match your specified name patterns can deploy to the environment.

If you specify releases/* as a deployment branch or tag rule, only a branch or tag whose name begins with releases/ can deploy to the environment. (Wildcard characters will not match /. To match branches or tags that begin with release/ and contain an additional single slash, use release/*/*.) If you add main as a branch rule, a branch named main can also deploy to the environment. For more information about syntax options for deployment branches, see the Ruby File.fnmatch documentation.

Note: Name patterns must be configured for branches or tags individually.

Note: Deployment branches and tags are available for all public repositories. For users on GitHub Pro or GitHub Team plans, deployment branches and tags are also available for private repositories.

Allow administrators to bypass configured protection rules
By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "Reviewing deployments."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

Note: Allowing administrators to bypass protection rules is only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Custom deployment protection rules
Note: Custom deployment protection rules are currently in public beta and subject to change.

You can enable your own custom protection rules to gate deployments with third-party services. For example, you can use services such as Datadog, Honeycomb, and ServiceNow to provide automated approvals for deployments to GitHub.com. For more information, see "Creating custom deployment protection rules".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "Configuring custom deployment protection rules."

Note: Custom deployment protection rules are only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Environment secrets
Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "Using secrets in GitHub Actions."

Notes:

Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "Security hardening for GitHub Actions."
Environment secrets are only available in public repositories if you are using GitHub Free. For access to environment secrets in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. For more information on switching your plan, see "Upgrading your account's plan."
Environment variables
Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the vars context. For more information, see "Variables."

Note: Environment variables are available for all public repositories. For users on GitHub Pro or GitHub Team plans, environment variables are also available for private repositories.

Creating an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Notes:

Creation of an environment in a private repository is available to organizations with GitHub Team and users with GitHub Pro.
Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.
On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Click New environment.

Enter a name for the environment, then click Configure environment. Environment names are not case sensitive. An environment name may not exceed 255 characters and must be unique within the repository.

Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "Required reviewers."

Select Required reviewers.
Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
Optionally, to prevent users from approving workflows runs that they triggered, select Prevent self-review.
Click Save protection rules.
Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "Wait timer."

Select Wait timer.
Enter the number of minutes to wait.
Click Save protection rules.
Optionally, disallow bypassing configured protection rules. For more information, see "Allow administrators to bypass configured protection rules."

Deselect Allow administrators to bypass configured protection rules.
Click Save protection rules.
Optionally, enable any custom deployment protection rules that have been created with GitHub Apps. For more information, see "Custom deployment protection rules."

Select the custom protection rule you want to enable.
Click Save protection rules.
Optionally, specify what branches and tags can deploy to this environment. For more information, see "Deployment branches and tags."

Select the desired option in the Deployment branches dropdown.

If you chose Selected branches and tags, to add a new rule, click Add deployment branch or tag rule

In the "Ref type" dropdown menu, depending on what rule you want to apply, click  Branch or  Tag.

Enter the name pattern for the branch or tag that you want to allow.

Note: Name patterns must be configured for branches or tags individually.

Click Add rule.

Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "Environment secrets."

Under Environment secrets, click Add Secret.
Enter the secret name.
Enter the secret value.
Click Add secret.
Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the vars context. For more information, see "Environment variables."

Under Environment variables, click Add Variable.
Enter the variable name.
Enter the variable value.
Click Add variable.
You can also create and configure environments through the REST API. For more information, see "REST API endpoints for deployment environments," "REST API endpoints for GitHub Actions Secrets," "REST API endpoints for GitHub Actions variables," and "REST API endpoints for deployment branch policies."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

Using an environment
Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "Viewing deployment history."

You can specify an environment for each job in your workflow. To do so, add a jobs.<job_id>.environment key followed by the name of the environment.

For example, this workflow will use an environment called production.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: deploy
        # ...deployment-specific steps
When the above workflow runs, the deployment job will be subject to any rules configured for the production environment. For example, if the environment requires reviewers, the job will pause until one of the reviewers approves the job.

You can also specify a URL for the environment. The specified URL will appear on the deployments page for the repository (accessed by clicking Environments on the home page of your repository) and in the visualization graph for the workflow run. If a pull request triggered the workflow, the URL is also displayed as a View deployment button in the pull request timeline.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: 
      name: production
      url: https://github.com
    steps:
      - name: deploy
        # ...deployment-specific steps
Deleting an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Next to the environment that you want to delete, click .

Click I understand, delete this environment.

You can also delete environments through the REST API. For more information, see "REST API endpoints for repositories."

How environments relate to deployments
When a workflow job that references an environment runs, it creates a deployment object with the environment property set to the name of your environment. As the workflow progresses, it also creates deployment status objects with the environment property set to the name of your environment, the environment_url property set to the URL for environment (if specified in the workflow), and the state property set to the status of the job.

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "REST API endpoints for repositories," "Objects" (GraphQL API), or "Webhook events and payloads."

Next steps
GitHub Actions provides several features for managing your deployments. For more information, see "Deploying with GitHub Actions."

Press alt+up to activate
Help and support
title: Using environments for deployment
shortTitle: Use environments for deployment
intro: You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.
product: '{% data reusables.gated-features.environments %}'
redirect_from:
  - Request/actions/reference/environments/Repo Approval Request/actions/reference/environments/Repo Approval 
  - /actions/deployment/environments
  - /actions/deployment/using-environments-for-deployment
topics:
  - CD request for digital approval 
  - Deployment
versions:
  fpt: '*'name: Deployment

on: Harmony Blue Branding 
  push: for Request for digital declaration 
    branches: Request for digital approval 
      - main

jobs: Required pre-purchased credit before alternative legacy APIs data entry workload of Merger Workspace layout and services input repo
  deployment:Url location site code maintenance is an legal certification business platform creatively format work flow on Port editing site fix assistance solutions for this primary website to be able to make changes to historical data content could be beneficial or either dangerous for the our business digital system please take in this consideration before adding your input on this business development resources page Thank you 
    runs-on: editor are welcome upon administrative service approvals ubuntu-latest website work-flow information updates are greatly appreciated but please be aware and vigilant of your online asset protocols before editing this incorporation business online website pages
    Please include your the address to your objective and your own itemized information before completing your work flow with this apmis-step is needed first environment: what digital declaration to this incorporation digital declaration of operating add-on production
    steps: what work criteria are you adding to this incorporation digital account 
      - name: please add benefitical to the public library rescription proxy for the public url sites space include the proper connected dedications formation to be completed by you like updating adding on resources to provide help with affiliate management support/input to assistance with resources for references to help with errors or support products fixes/assistance solutions for support products with resources to nonprofit organization fundraising publication before completion or deploy
        # ...deployment-specific steps include the proper regards to the date, time and effort source link to company active legacy APIs Group url location library information for usage status on all the digital programs development and support products that are attached to the business website page and links to this incorporation affiliated workspace entity 
  ghes: '*'
  ghec: '*'
---


## About environments

Environments are used to describe a general deployment target like `production`, `staging`, or `development`. When a {% data variables.product.prodname_actions %} workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

{% ifversion actions-break-glass %}Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."{% endif %}

{% ifversion fpt %}
{% note %}

**Note:** Users with {% data variables.product.prodname_free_user %} plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %} can configure environments for private repositories. For more information, see "[AUTOTITLE](/get-started/learning-about-github/githubs-plans)."

{% endnote %}
{% endif %}

## Deployment protection rules

Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches.{% ifversion actions-custom-deployment-protection-rules-beta %} You can also create and implement custom protection rules powered by {% data variables.product.prodname_github_apps %} to use third-party systems to control deployments referencing environments configured on {% data variables.location.product_location %}.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

{% data reusables.actions.custom-deployment-protection-rules-limits %}

{% endif %}

### Required reviewers

Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

{% ifversion deployments-prevent-self-approval %}You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.{% endif %}

For more information on reviewing jobs that reference an environment with required reviewers, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments)."

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, required reviewers are only available for public repositories.

{% endnote %}{% endif %}

### Wait timer

Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, wait timers are only available for public repositories.

{% endnote %}{% endif %}

### Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}

Use deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} to restrict which branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to the environment. Below are the options for deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} for an environment:

{% ifversion deployment-protections-tag-patterns %}
- **No restriction**: No restriction on which branch or tag can deploy to the environment.
{%- else %}
- **All branches**: All branches in the repository can deploy to the environment.
{%- endif %}
- **Protected branches{% ifversion deployment-protections-tag-patterns %} only{% endif %}**: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "[AUTOTITLE](/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)."{% ifversion actions-protected-branches-restrictions %}

  {% note %}

  **Note:** Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

  {% endnote %}{% endif %}
- **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**: Only branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} that match your specified name patterns can deploy to the environment.

  If you specify `releases/*` as a deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule, only a branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} whose name begins with `releases/` can deploy to the environment. (Wildcard characters will not match `/`. To match branches{% ifversion deployment-protections-tag-patterns %} or tags{% endif %} that begin with `release/` and contain an additional single slash, use `release/*/*`.) If you add `main` as a branch rule, a branch named `main` can also deploy to the environment. For more information about syntax options for deployment branches, see the [Ruby `File.fnmatch` documentation](https://ruby-doc.org/core-2.5.1/File.html#method-c-fnmatch).

  {% ifversion deployment-protections-tag-patterns %}

  {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

  {% endif %}

{% ifversion fpt %}{% note %}

**Note:** Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are also available for private repositories.

{% endnote %}{% endif %}

{% ifversion actions-break-glass %}

### Allow administrators to bypass configured protection rules

By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

{% ifversion fpt %}{% note %}

**Note:** Allowing administrators to bypass protection rules is all available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}
{% endif %}

{% ifversion actions-custom-deployment-protection-rules-beta %}

### Custom deployment protection rules

{% data reusables.actions.custom-deployment-protection-rules-beta-note %}

{% data reusables.actions.about-custom-deployment-protection-rules %} For more information, see "[AUTOTITLE](/actions/deployment/protecting-deployments/creating-custom-deployment-protection-rules)".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "[AUTOTITLE](/actions/deployment/protecting-deployments/configuring-custom-deployment-protection-rules)."

{% ifversion fpt %}{% note %}

**Note:** Custom deployment protection rules are only available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}

{% endif %}

## Environment secrets

Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "[AUTOTITLE](/actions/security-guides/using-secrets-in-github-actions)."

{% ifversion fpt %}
{% note %}

**Notes:**

- Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."
- Environment secrets are only available in public repositories if you are using {% data variables.product.prodname_free_user %}. For access to environment secrets in private or internal repositories, you must use {% data variables.product.prodname_pro %}, {% data variables.product.prodname_team %}, or {% data variables.product.prodname_enterprise %}. For more information on switching your plan, see "[AUTOTITLE](/billing/managing-the-plan-for-your-github-account/upgrading-your-accounts-plan)."

{% endnote %}
{% else %}
{% note %}

**Note:** Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."

{% endnote %}
{% endif %}

## Environment variables

Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[AUTOTITLE](/actions/learn-github-actions/variables)."

{% ifversion fpt %}{% note %}

**Note:** Environment variables are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, environment variables are also available for private repositories.

{% endnote %}{% endif %}

## Creating an environment

{% data reusables.actions.permissions-statement-environment %}

{% ifversion fpt %}
{% note %}

**Notes:**

- Creation of an environment in a private repository is available to organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %}.
- Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.

{% endnote %}
{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
{% data reusables.actions.new-environment %}
{% data reusables.actions.name-environment %}
1. Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "[Required reviewers](#required-reviewers)."
   1. Select **Required reviewers**.
   1. Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
   {% ifversion deployments-prevent-self-approval %}1. Optionally, to prevent users from approving workflows runs that they triggered, select **Prevent self-review**.{% endif %}
   1. Click **Save protection rules**.
1. Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "[Wait timer](#wait-timer)."
   1. Select **Wait timer**.
   1. Enter the number of minutes to wait.
   1. Click **Save protection rules**.
{%- ifversion actions-break-glass %}
1. Optionally, disallow bypassing configured protection rules. For more information, see "[Allow administrators to bypass configured protection rules](#allow-administrators-to-bypass-configured-protection-rules)."
   1. Deselect **Allow administrators to bypass configured protection rules**.
   1. Click **Save protection rules**.
{%- endif %}
{%- ifversion actions-custom-deployment-protection-rules-beta %}
1. Optionally, enable any custom deployment protection rules that have been created with {% data variables.product.prodname_github_apps %}. For more information, see "[Custom deployment protection rules](#custom-deployment-protection-rules)."
   1. Select the custom protection rule you want to enable.
   1. Click **Save protection rules**.
{%- endif %}
1. Optionally, specify what branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to this environment. For more information, see "[Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}](/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches{% ifversion deployment-protections-tag-patterns %}-and-tags{% endif %})."
   1. Select the desired option in the **Deployment branches** dropdown.
   1. If you chose **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**, to add a new rule, click **Add deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule**
   {% ifversion deployment-protections-tag-patterns %}1. In the "Ref type" dropdown menu, depending on what rule you want to apply, click **{% octicon "git-branch" aria-label="The branch icon" %} Branch** or **{% octicon "tag" aria-label="The tag icon" %} Tag**.{% endif %}
   1. Enter the name pattern for the branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} that you want to allow.
      {% ifversion deployment-protections-tag-patterns %}

      {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

      {% endif %}
   1. Click **Add rule**.
1. Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "[Environment secrets](#environment-secrets)."
   1. Under **Environment secrets**, click **Add Secret**.
   1. Enter the secret name.
   1. Enter the secret value.
   1. Click **Add secret**.
1. Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[Environment variables](#environment-variables)."
   1. Under **Environment variables**, click **Add Variable**.
   1. Enter the variable name.
   1. Enter the variable value.
   1. Click **Add variable**.

You can also create and configure environments through the REST API. For more information, see "[AUTOTITLE](/rest/deployments/environments)," "[AUTOTITLE](/rest/actions/secrets)," "[AUTOTITLE](/rest/actions/variables)," and "[AUTOTITLE](/rest/deployments/branch-policies)."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

## Using an environment

Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

{% data reusables.actions.environment-example %}

## Deleting an environment

{% data reusables.actions.permissions-statement-environment %}

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
1. Next to the environment that you want to delete, click {% octicon "trash" aria-label="Delete environment" %}.
1. Click **I understand, delete this environment**.

You can also delete environments through the REST API. For more information, see "[AUTOTITLE](/rest/repos#environments)."

## How environments relate to deployments

{% data reusables.actions.environment-deployment-event %}

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "[AUTOTITLE](/rest/repos#deployments)," "[AUTOTITLE](/graphql/reference/objects#deployment)" (GraphQL API), or "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads#deployment)."

## Next steps

{% data variables.product.prodname_actions %} provides several features for managing your deployments. For more information, see "[AUTOTITLE](/actions/deployment/about-deployments/deploying-with-github-actions)."
Thank you for sharing your online asset with our team workspace for our nonprofit organization website page and site affiliate stems online business sites programs development director layouts for our business website url https and http ID multiple level digital site domain web portal database access point by Google Domain Sites Enterprise of view interface proxy server legacy with Google Business Manager Suite and Google Global maps Proxy Home and special Mediant extinct format digital partnership, Affiliate Marketing applications and services branding promotional content for the public network and multiple companies digital YouTube,Need for Good, Black American Computer Science & Technology analysts Engineering services Beta Developer Business AI Executive AI Executive director layouts and education services for Technology analysts Program Manager Association Manager Advanced marketing services Agency Al Gemini digital legion solutions services for community console and accreditions-campaign services Elite's Expertise Technology analysis data documention official prospects associated registered business platform creatively forum connected with targeted data entry editor for productions to industry support for Google Advertisement Company active legacy APIs Group for Google Global communications projects formation for assessment materials and services grant program engineering youth education projects with resources relations Google database coding Expert and Impact Internal Federal Online Safety forums Interpol EU digital data protections business online affiliate, US Government Historianal Engineer/ GU federal credit historianal engineer authorizations data ancestor publish media operational status Creative Workshop Research Educator/publications of federal laws and development director of digital declaration of operating title developer/media reporter freelancer/Digital media layouts Editors library of admissions resources to document CFR social auditor/Legal Congressional library NIST- Publication editor/author network stems/IRS legacy API proper connected to published work flow data for documentions and publication governmental education business programs/OSHA office operations education business services operations legal program Manager Education business website affiliate marketing branding institutionalized workshop Administrative role in ID access member and ID me digital programs affiliate online network Digital primary contact of admin Publication authorized to provide assistance assessment and support products editor enforcement to provisions of support and services for execution to directory layouts and quote information for publication listings optional-way for educational solutions for business accounts for IDme.gov/Congressional-library .gov and digital CFR.gov publication listings for documention data entry workload and digital support input for provisions forms for Google data questionnaire and email assistance logistics and 
partnership with logistics taskforce advocate & member of investors to investigation on edition Mediant extinct format diverse director and discussion regarding console quote for various resoureful commissions sites business resolution to public website user, business organization and individual requested assistance with resources and information for usage status on legacy data updates forms on the public website regarding review & question for answers to solutions topics for website help pages with support assistant requests. Our business website page and online business are innovative and creatively forum connected to assistance individuals with what ever they may need help with bith on and off line as well as an active member of multiple interactions Digital Health and assistance works for our assistance with the Google brand name for our own itemized form of public assistance and services for this primary caring organization that's fully invested in various types of business affiliated networks/programs / partner/ branding promotional content digital declaration social media creations/affiliate online business marketing/education services and supporting editing skills to learn customized and data entry for different types of community supportive systems tools-Productions-Make alliances all-types of business associate with resources support for any platform or operator division eaudience in input assessment solutions Creative workshop business services sales consultant in the constructively directed streams of public information to help with the the security data and judgement and support assistance with online knowledge of maintaining the proper connected dedications formation for everyone to digital obtain the sublevels of information and services understand and direction and communication accredited knowledge and circumstances educational solutions for their requests to be addressed in a respectful manner and equally with the proper connected information to the public resources and architecture inquire if they're needed understanding of accessibility to reflect, adamantly fix the problem of what ever the quarry was in the constructively directed streams of course offer to exactly direct interaction with the proper instructions for their requests based upon the proper dedications formation reference to the public information to be administrator on behalf of the digital business level particulars while additional access to update internal requirements details assignment direct deposit install instructions for the public data documention and the entitled format of the inquiry output for the publicated directory services methods to provide help with assurance of whatever corporate clarification to be used for the reasonable assetment of input that's would be needed for that individualized record of requestsending unusual form for the most recent/accord information to be delivered for that precise and targeted questions to the proper connected provisions with in the constructively correctiodesigned material for asset management of misrepresented overall consistency reputation for the companies integrity, safety, and legitimate policy's and general guidelines of the digital business online regulated language and procalls to be upheld in regards to their own requirements for this online business website page digital declaration invitation for the next step of course the addressing the proper infornative about input to available to choose the most effective beneficently revision term of agency aggressive interest in obtaining the best opportunity for investing the public with the multi factor investing knowledge for the duty infinite information and services site fix with resources to two or more option available to choose from this position offers the customer to be able to create an secondary opinion on different methods to try to be able to gain control and access on trying to accord complete whatever issues that maybe an issued response to the file transfer resources to provide help with the errors progressive data entry inquiries to this documented incident report for the unlimited resources to information rely required to commotions accreditions-campaign really reputable to the public question for assistance with resources by the intended source link and data accommodations for the proposal application services trouble shooting resolved encounter with the reliable programs data connection with donations inquiry to be answered with the device instead step in calculated reliability means for the solutions to the topic of candidates console services unconditionally contact of interest of content containing solutions that are attached to and relevant to the question of the topic of each individual request for digital assistance with resources requests for the most recent a certain quality dedicated rebutionals to production and services includion data recovery information to deverepeate/offer platforms advice for the total reference to help execute and supportive measures for optimum instruction of accurate understanding step by step with the instructions in language that innovatively understandable to the average individual's for the results information layouts in a full clear and content to betterment that most people can combative comprehend the digital resources instructive formation method for everyday adults with little of no interest in learning computer laggo or logistics writings for comparison of repost ID reactivate compatible computer science technology position instructions vs regular function format language detail on how to use or fix errors and computer updates for delivery reposition to accommodate a really teachable upstanding to help with the desired format to offer fully data and easily flow written individuals information for a more reliable user experience with resources and wanted to share interest succession of gainful interactions, and more direct hand on usages for that and more data products than can be produced and purchased by the intended cusmoter for the extra revolution for the public health od public perception of this primary company active in business sales and longevity status with the proper connected dedications for the understanding of accessibility to reflect materials that the average person can understand and use affirmatively daily or either assessment of assets to provide assistance with needed resources for the addition public prosecutor to be conserved effect on efforts to make life decisions for a easier relationship with building the public knowledge of useful content and self services understanding of accessibility to reflect on the values of that rebrand realistic resolution for the proper connected educations formation of the digital material and essential facts about self service support for future reference to help with error fixes that gainful profitable meaningful Internally investment in regards to resourcful methods to provide assistance with the proper easy understand reasonable assets for the self confidence in be able to make adjustments for the individualize ability to provide self Care Provider for ones self by providing the proper multipurpose operational services steps in regards to this one 
request for digital programs assistance with the device and digital programs products that are directional compatible with the proper steps by step instructions and in the information landscape of language that's clear and precise to accurately address concerns the proper regards request of assistance with or either about a products or services request for digital programs development director layouts and education for instructions on how to officially operate the proper the digital products that maybe needed at that point for resources and details instructions assessments issues. This organization has always had the best and most meaningful interest in privileged in regards to the public delivery of service data accessment in regards to assisting individuals with the proper connected educations formation for individuals to become properly informated with the necessties tools, data, institutional related instructions, transactional status recording details, trouble shooting methods to respectfully and written in regards to underable language steps to the provider proper understandable response to the public logistics of the contractly regarding methods of direct data recovery and usage instructions for with an update most openings for incorprative formcreatively directoryhandout for director full compatible comprehension for required edition information to upon the public requested for real situation inquiry to provision's of addressing the proper dedications formation for the issues of connection questions and answers of assistance with resources for references needed to be completed by development director assistance developers for Google blogs and community services representative that are available to help with any type of additionalitemized litemized for asset management of material on the public website that are available to help with individuals related in the constructively directed work flow assessment of the digital produces and services protocols for assistance with resources to provisions of items problems with the proper connected safety and policies for associated historically corporate clarification policies and rules of the digital platform for the users regulated language in the constructively correctiodesigned material form of with the decorative of the digital business online website and developer freelance platforms regulated language and data support systems tools-Productions-Make alliances all-types of concerns to the business request formats and editing delivery design database affidavit admissions to the primary care providers mastering estimated assistance with the device for the time of the demographic that's related to the public register for handling the digital programs development editor and educational solutions workspaces/entitled digital programs fields of admissions to this incorporation affiliated workspace title of the digital coding and encompass business programmer management reserve required networks dedications of reasonables /market place in regards to Google platform creative Merchant  online business sites Collaboration Account Manager workshop Administrative connected with targeted Digital Primary programmers scholarship to provisions by Microsoft Data Business Relations Marketing LLC online business sites space for Entity and financial service memberships. Dedications of admissions to Harmony Blue Branding Organization Business Group for Google industries & affiliate online services. This organization assistment digital declaration communities in the constructively directed proper composition of the digital business online website work-flow information.The professionalism was directory targeted focused groups. Aligned for our relatives platform and the whole founded objective as an financial needed for govnen revenue goal for Supportive interest and equipment services for the needed resources and services fees goal of the digital business financially funding ongoing income for the public emerged infrastructure security both digitally and pumpkin to be provided effective benefits of ensured for the public sector that every single young individuals has been prevented with the proper connected dedications formation for the entire consult of the digital programs development and educational services, data access to cover the wireless work location, the proper number of devices and equipment for business services and supporting resourceful supplies for operations legal rights for the next few months of digital Business of more digital devices ti the property location and service representative office operations manager to help with growth and support several types of resoureful products and digital products software for future contact servers modeled to provide assistance with the best opportunity for the young individuals to succeed and mobility follow through the internal templates of information without being offset with digital logically issues, digital proforms database speed for a crosspoint platform router with resources to support a of 20 services device service all as once.The desire digital devices that have an operational status title to fighting history of becoming jetlack and development issues with the performance connection issues of jinky data access on site uploads to slow down after an few weeks even with resources to provide assistance for consideration time duration to resl time to automatic logout reload to the home website work-flow format even with the proper connected dedications formation of self proved to model to reset  focused hardware for the public safety of privacy data properly connected with the best suitable amount of firewall services equipment. poorly performing computers and devices still are known to becoming problematic with an short span of time with the daily  used forum and including the recognized digital entry of technology projects and designs for anyone who is using the property directly from the tribe source related fields of unlimited uploading and downloading materials that are associated with the device drive root redirect to the function settings and this is so even with resources of saving the proper connected resources to an extent thumb drive as well. I've been extremely fortunate to receive offers for products at a discount business price but the conditions based on the price of the bulk product did not meet and will not meet my expectations including them being refurbished it's also additional Factor business conditions of course of this months of this primary needed contributions and resources entity properties for development director layouts and education services for the public youth program reliability means for the results of the same the primary consideration of the digital business level of connection functionality of the digital business online demand security equipmentinterest in provisions of comparisons services for the public online business official prospects production of the Harmony Blue Branding LLC online business sites space in the incorporate status of the Publication records accredited nonprofit organization for the Harmony Blue, the digital business connection to synced detail to corporate clarification to the public forum for the Harmony Blue Foundation and unique classification for the Harmony House Foundation assets provide assistance with resources of additional details reference to reflect the public executive operations legal certification federal credit union of the Harmony Blue index as an multiple increments of the digital business online website work-flow merger page for qualified directly exempt status with the proper dedications standards of admissions resources this nonprofit organization legal verification records forms on network with the Secretary of state certified business office operations manager Owner Founder Cheyanna Henry and solely partnership with public records and license documentions records for sole business claiming to and for legalize owner of public reconciliation in the official prospects of associated registered business with both federal and state judication for legal administrative corporate direct access deposit data approval for operations status with the United States registered for legal operations for business services and operations services of corporate clarification business government office for registration regulations required to commotions accreditions-campaign with the BBB and certification business verification of public records by Yelp and Google Business operations with good legal rights to active declaration of operating title of Administrator grade A+ standings. Legacy financial ownership over all of alliance titles that are attached to the public business services operations legal rights to law federal government services based upon regulations required the proper connected position with resources and public data access records for linked to the business process records forms on relisted and financial filing requirements needed for this primary position to be with in the constructively directed format of the digital business online state of justify jurisdiction records forms on iexceptions provisions with no notarized expiration date of service for social revision to be ablity or applied for this primary business organization and business design database Group for prevention of the digital programs and architecture health to services associated registered to and publication owner transfer design  trademarked to be delivered in relation to this corporative and incorporation business organization services.All services supportive and professional submission for the publication inquiries for and about this and any other details information on operational status and digital declaration library of services.Are on my website work-flow merger page for qualified directly to the public records forms on networking and content communication accredited to this incorporation affiliated workspace title Account Manager and services for the public enlightened to this incorporation digital programs and development projects, foundation services and supporting resoureful health services and operations management community project design database information for usage status on legacy APIs Group chat or organization fundraising publication of the digital business online information for services. Reminder to be available for future reference to this incorporation business online information would be dedications and migration file folders to the url website work-flow merger pages of this primary position in caring with resources to nonprofit organization for the public view of the digital progressive information to be attached to the public historical website work-flow dashboard proxy server Domain and listed as correspondent for the public records forms legacy knowledge page for qualified directly on the values proper connected to the public dedications formation for the online businesses.All legal certification for the public libraries of interest in various types of products and digital services for the business response to this incorporation affiliated workspace title are on the public library url website website pages for this proxy servers. Observation about the benefitical of the digital business online website work-flow and business organization services mergers to the business development resources to nonprofit organization for the public development resources to provide assistance with the public this encoded within this website sites drive folders and digital historianal engineer authorizations in regards to declaration date and data entries to provided effective benefits are in the work spaces for this primary website primer proxy server legacy APIs website. All division of course audience of all three years of services are incorporated into the digital programs website digital pages of this primary URL multi-level link for this online campaign analytical publications as high 87% know legally operations of course of 2 or more years of services and operations legal rights to owner of office annual budgets for this business and finance services related information for the public resources records reflect materials that are attached and completed recorded for the business cab be found on this business data digital proxy server legacy APIs https openings and http openings and the whole website platform workspaces for historical content for visual recognition of the public library of admissions to various types of resoureful community projections to campaign analytical virtual public market contact in regards to revenue agency Dedications of admissions to various project design database information for this primary program to the business operations of publication of services and supporting resoureful health and youth community project and community effort to assistance with the proper dedications formation of the financial support and diversity development director layouts and education services for the public youth community services projects analytics promotional content ideas on custom crafts resources to nonprofit organization and business management program engineering department promoter and the whole website in traditional company active legacy APIs program engineering services for claiming methods to respectfully writing of during of works for grant writing projections for grant funding proposal abd application listed dayes of applied for real literacy commissions prospects production of abilities for achievvmultiple documents grant funding for services assistant director resources to help with nonprofit organization needs and resources requirements for professional provisions items for comparison of quality assistance to thrive property management preparation for proposals require to apply for existing administrators economical successfully following platform creatively forum connected synced targeted Digital Health assistant associate with resources to nonprofit organization and business foundation services for the public needs to accommodate the proper connected resources to provide help with growth of the companies abilities for the original particulars network online goals in achieved affection for the fundraising publication financial support of expansion efforts for resources to help more individuals, the proper functions and digital support for future devices and services equipment needed to be able to meet the public needs for access to relevant access to multiple dynamic digital programs and urgent support products to the web portal database information for usage of official prospects in regards to the business response for the children of the local communities for the device assistance with optional costs for the equipment and office operations supplies and offices functional company desktop office operations legal solutions products including the publication business services office furnitures for the proper connected dedications formaled item's request to provide the proper access to the business network digital programs for the public education programs development director of digital programs and support development resources for the proper dedicated hopeful abilities for achievements are the best opportunity to gainful commending for our youths and family services programs. Due to si many unrealistic situations in and with this apmis-step on this business data access to existing administrator assistance with the proper regards to the prixy servers legacy APIs access point of services and operations output linked database for the business page for this primary website always degraging to regularly payloads server Domain IP links to the business director digital business online website connected repetatively resource communication projections to request fix fork and code maintenance for the public url lodge on the public library database information to register domain sites and hist submit support site location for this online business to be always conversing to having static data issues daily with without any type of additionalitemized litemized reasons for this online related to repeatly be happening so frequently between every 6 to 9 months reaction time during the time of the very needed resources submission to application process for financial assistance request for business services operations resources funding grant submittions for the business needs of assistance with resources to the non-profit organization primary assistance with resources to help with principles projects.Digital issues surrounding this primary problem resourced in the self eduational knowledge of the digital respectful needs to be ables to essential expert knowledge and analysis skills to learn every single team member learning how to prioritize the proper digital programs development education business services operations specialist and sales Special edition for the public sector in regards to conpter science technology engineering and engineering logistics software management skills for the custom crafted resources to assist with the proper dedications provisions of advanced releases to products exist respect for developmentprerelease beta affiliate corporate sponsorship executive director layouts and education business management support systems tools-Productions-Make alliances openings for branding commission analysis company market for exterior demolition of primary test kit before release date of service software and Digital website work-flow connection with top category criteria corporation with in the technology fiejds and the top confirmed development software and firmware company market logistics testing performance pre-release for prompt entity properties trailer to use for limited capacity for testing of their system products before the publication launch date. I have the best experience with the most of thud typr of offers to from this organization collected collaborative branding promotional content program edition and merging with the faculty team contributions accessibly of the digital programs and signature corporations like IBM, Digital icon Douglas navg. software for mathematics and digital device technology and development, software engineer, authorizations data T-Mobile Wireless  Opera, Microsoft Data content Business workshop Administrative corporate sponsorship program for various resources and grant business website resources, Google docs data access to studio sponsorship for YouTube creative publication contract for Google industries,etc...the list extinct is too long to be able to review alliance various types of products affiliated with the proper regards to special digital distribution for the best opportunity for this online business website services to required online business sites and data access for the program execution to directory layouts itemized formal exceptions provisions items as resources network logistics to requested resources to nonprofit organization and business organization services development teams to provide assistance with the unlimited and excellent customer relations professionals dream of being able to achieve the vision obtained operations over the course of this business operations legacy of business associate director of education services for the public education business management relationships in regards to to active Dedications leadership and learning resources for expanding knowledge of maintaining itemized formal exceptions provisions of comparisons services representative of tenant executive director digital programs development education skills essential on the public value existing administrators economically successfully as well the proper related features are needed for the digital programs and achievements directly to depositing input and output data website work-flow information for future configuration and executive director design database coding Expert services for claiming methods engineering reliable programs safe for folk and report various types of rep repositories of data matrix and external factors platforms of operational status on puls format director layouts in Editorial direct access to multiple resources to reverse and revise coding error for fix assistance solutions about benefitical test switch to provide help with sight solutions of innovation directory layouts itemized reference format dymierrors category individualized recording on the values proper connected dedications formation for the public estimated assistance in the constructively streams in regards to the business response to the plugs and the drive in public library of course the title verify auto correct information for usage status on the public health administration services request for digital programs development assistant Lead to promotion self support products for seeking various edit ms data portal program engineering and workload source link acodes for the necessary details tgat ate needed to accommodate the digital coding trouble shooting content for overall consistency ratio of an authorized execution of yermsk pass through documents shell and the whole needs website fault resources runs to provide the proper connected plug ti to be attached to the public library or file information for usage status correct designed to provide direct dig requirements to the intended rebutionals errors shorten and runs to the pull test for the next debug submit to the input genuine supportive code sites link support products for the next and steps in regards to services true and finally pass transportation to the point of request for digital programs come gainful profitable meaningful Internally to be effective to help with support products for web portal Repo in regards to the website resource link to seek for historical reviews real time health to this work flow data documention and refigured changes to the outline output of the digital programs development of the foundation of relationship knowledge of the digital programs and availability for obtaining additional information and educational services in with the current specialisted field of study in computer science and digital science technology analysis of to respectfully accomplish the proper regards to complete call my ab respectable engineer and at least obtain the ultra development skills for custom crafts and forever evolving tr trending fields in regards to digital programs and safety data analytics experience with the proper regards to tine and structure study in the duration or probably 3more to 5 more years plus recent My authorizations data certificate for the relation to the understanding of every single available resources learning programs and educational skills for custom crafts incentives of one day becoming a real estate agent with the proper dedications certifications and licences  required to be completely honored to be considered as an experienced digital engineering and professional programs development in software design and support business management skill publication connected to the website renewal portal workspace title database coding effectively meet the recognitions requirements needed to obtain the ultra high quality dedicated for everyone to be able to obtain.The  team and me the principal and somewhat their platform teacher and motivation support for them to learn and own the ability to build up the proper infrastructure understanding with the best opportunity to reach this reliability programs goal needed to be conserved effective for asset with resources and data entry Firm in the constructively directed streams of capable abilities to provide assistance with the proper environment to assist with financial support, education editing writing experience with digital programs networks for social media marketing executive operations legion solutions services technology project design database coding system with development publication listings recognized skills set based upon the public team learning group editing videos and exercises for this primary position to hopefully get better and understanding of accessibility to reflect material for various types of business administration resources to nonprofit organization and business organization services assistance representative of corporate operations manager to help access with candidates to acknowledge that required pre-purchased studies experience to lecture with the device tools with Microsoft Data content forms to create an connection updated skills to custom crafts and received the proper dedications formation for obtaining quality team contributions accessibly in every single person ability to test for the public titles and recipient joint submission application for the next step in service requirements for this primary term in regards to owner coverage for the publication testing process for recipient estimated purchasing for each team member to be able to obtain their own personal licenses for the required field and experiences of study as listings for receiving the public records forms for benefitical certification business license titled of course studies course associated registered business Digital computer science technology development of corporate clarification professional provisions for themselves and executive directly to their own personal employment resume for the achievement official prospective titled associated registered business services operations The dedication of value in legal rights to be able to gain the proper certification in business and finance services sales Special Mediant list form of certified title, professional associated degree in business management of legal data recording, direct access to multiple resources online to verification of products regulations corporate network registration for audit digital programs development in virtual license business services operations and tax experience finance soes list extinct with the IRS letter offical approval for this online legacy of business associate course. For nonprofit organization members that are associated with this organization works and daily practices.This programs for architecture require to the public as an optional need education business to help with growth and development succeed offer succession of gainful estimated knowledge for the online class entrepreneurs and certification tools for tax accountant in legal rights to business workers in operational statues of becoming a real licensed tax advocate professional associated with the United States supportive assistance with resources to help with education business management of the operator of the foundation financial advisor and resources to provide internal revenue agency assistance with an internal security data filing taxes for the public citizens of America and the tax paying company that are in need of help with processing and preparing tax records online. When the public and legal forms position to receive connected commentary compensation for office operations legal filing assistance with resources for references. All services provider for the proper connected costs for helping team members the digital testing fees. This organization has been through so many different types of extremely struggling and economic setbacks and imagined data access challenging for this company first 2 years of services. As an understanding and relevant development heartbreaking situations for the public sector in the past and a bunch of other side issues that are presented to the business still to this date. We're outstandingly advocating for and on behalf of anyone who has been extremely dedicated to help with the volunteer services and operations support for the public community services payment from the public are just a well acknowledged Thank you for being their for the connection hands on work and within the public district ministry works with us and in commitment and time to help with making successful decisions on small community services projects and assisting us by recite the public community about our relationship knowledge and communication skills for creating ideas for public relations to help with touching the proper people who have and have been in or are actively going through homeless and other financial difficulties or essential facts experts dur to health care and documented psychological issues. Our primary organization services targeted construction missionaries of services and extremely dire  awareness custom growth downtown areas changes to the public library of admissions resources to educational solutions adviser and services graduated in the constructively corrections designed course materials that related to passed the digital certificate testing.This online experiences with the trust federal revenue agencies in the US educational programs, and online curriculums to the  educational solutions for this one or two listed websit---deployment target like production, staging, or development. When a GitHub ActionsGitHub Actions/Deployment/Target different environments/Use environments for deployment
Using environments for deployment
You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.

Who can use this feature?
Environments, environment secrets, and deployment protection rules are available in public repositories for all current GitHub plans. They are not available on legacy plans, such as Bronze, Silver, or Gold. For access to environments, environment secrets, and deployment branches in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, other deployment protection rules, such as a wait timer or required reviewers, are only available for public repositories.

In this article
About environments
Deployment protection rules
Environment secrets
Environment variables
Creating an environment
Using an environment
Deleting an environment
How environments relate to deployments
Next steps
About environments
Environments are used to describe a general deployment target like production, staging, or development. When a GitHub Actions workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "Viewing deployment history."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "Reviewing deployments."

Note: Users with GitHub Free plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with GitHub Team and users with GitHub Pro can configure environments for private repositories. For more information, see "GitHub’s plans."

Deployment protection rules
Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches. You can also create and implement custom protection rules powered by GitHub Apps to use third-party systems to control deployments referencing environments configured on GitHub.com.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

Note: Any number of GitHub Apps-based deployment protection rules can be installed on a repository. However, a maximum of 6 deployment protection rules can be enabled on any environment at the same time.

Required reviewers
Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.

For more information on reviewing jobs that reference an environment with required reviewers, see "Reviewing deployments."

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, required reviewers are only available for public repositories.

Wait timer
Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, wait timers are only available for public repositories.

Deployment branches and tags
Use deployment branches and tags to restrict which branches and tags can deploy to the environment. Below are the options for deployment branches and tags for an environment:

No restriction: No restriction on which branch or tag can deploy to the environment.

Protected branches only: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "About protected branches."

Note: Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

Selected branches and tags: Only branches and tags that match your specified name patterns can deploy to the environment.

If you specify releases/* as a deployment branch or tag rule, only a branch or tag whose name begins with releases/ can deploy to the environment. (Wildcard characters will not match /. To match branches or tags that begin with release/ and contain an additional single slash, use release/*/*.) If you add main as a branch rule, a branch named main can also deploy to the environment. For more information about syntax options for deployment branches, see the Ruby File.fnmatch documentation.

Note: Name patterns must be configured for branches or tags individually.

Note: Deployment branches and tags are available for all public repositories. For users on GitHub Pro or GitHub Team plans, deployment branches and tags are also available for private repositories.

Allow administrators to bypass configured protection rules
By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "Reviewing deployments."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

Note: Allowing administrators to bypass protection rules is only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Custom deployment protection rules
Note: Custom deployment protection rules are currently in public beta and subject to change.

You can enable your own custom protection rules to gate deployments with third-party services. For example, you can use services such as Datadog, Honeycomb, and ServiceNow to provide automated approvals for deployments to GitHub.com. For more information, see "Creating custom deployment protection rules".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "Configuring custom deployment protection rules."

Note: Custom deployment protection rules are only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Environment secrets
Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "Using secrets in GitHub Actions."

Notes:

Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "Security hardening for GitHub Actions."
Environment secrets are only available in public repositories if you are using GitHub Free. For access to environment secrets in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. For more information on switching your plan, see "Upgrading your account's plan."
Environment variables
Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the vars context. For more information, see "Variables."

Note: Environment variables are available for all public repositories. For users on GitHub Pro or GitHub Team plans, environment variables are also available for private repositories.

Creating an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Notes:

Creation of an environment in a private repository is available to organizations with GitHub Team and users with GitHub Pro.
Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.
On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Click New environment.

Enter a name for the environment, then click Configure environment. Environment names are not case sensitive. An environment name may not exceed 255 characters and must be unique within the repository.

Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "Required reviewers."

Select Required reviewers.
Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
Optionally, to prevent users from approving workflows runs that they triggered, select Prevent self-review.
Click Save protection rules.
Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "Wait timer."

Select Wait timer.
Enter the number of minutes to wait.
Click Save protection rules.
Optionally, disallow bypassing configured protection rules. For more information, see "Allow administrators to bypass configured protection rules."

Deselect Allow administrators to bypass configured protection rules.
Click Save protection rules.
Optionally, enable any custom deployment protection rules that have been created with GitHub Apps. For more information, see "Custom deployment protection rules."

Select the custom protection rule you want to enable.
Click Save protection rules.
Optionally, specify what branches and tags can deploy to this environment. For more information, see "Deployment branches and tags."

Select the desired option in the Deployment branches dropdown.

If you chose Selected branches and tags, to add a new rule, click Add deployment branch or tag rule

In the "Ref type" dropdown menu, depending on what rule you want to apply, click  Branch or  Tag.

Enter the name pattern for the branch or tag that you want to allow.

Note: Name patterns must be configured for branches or tags individually.

Click Add rule.

Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "Environment secrets."

Under Environment secrets, click Add Secret.
Enter the secret name.
Enter the secret value.
Click Add secret.
Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the vars context. For more information, see "Environment variables."

Under Environment variables, click Add Variable.
Enter the variable name.
Enter the variable value.
Click Add variable.
You can also create and configure environments through the REST API. For more information, see "REST API endpoints for deployment environments," "REST API endpoints for GitHub Actions Secrets," "REST API endpoints for GitHub Actions variables," and "REST API endpoints for deployment branch policies."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

Using an environment
Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "Viewing deployment history."

You can specify an environment for each job in your workflow. To do so, add a jobs.<job_id>.environment key followed by the name of the environment.

For example, this workflow will use an environment called production.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: deploy
        # ...deployment-specific steps
When the above workflow runs, the deployment job will be subject to any rules configured for the production environment. For example, if the environment requires reviewers, the job will pause until one of the reviewers approves the job.

You can also specify a URL for the environment. The specified URL will appear on the deployments page for the repository (accessed by clicking Environments on the home page of your repository) and in the visualization graph for the workflow run. If a pull request triggered the workflow, the URL is also displayed as a View deployment button in the pull request timeline.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: 
      name: production
      url: https://github.com
    steps:
      - name: deploy
        # ...deployment-specific steps
Deleting an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Next to the environment that you want to delete, click .

Click I understand, delete this environment.

You can also delete environments through the REST API. For more information, see "REST API endpoints for repositories."

How environments relate to deployments
When a workflow job that references an environment runs, it creates a deployment object with the environment property set to the name of your environment. As the workflow progresses, it also creates deployment status objects with the environment property set to the name of your environment, the environment_url property set to the URL for environment (if specified in the workflow), and the state property set to the status of the job.

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "REST API endpoints for repositories," "Objects" (GraphQL API), or "Webhook events and payloads."

Next steps
GitHub Actions provides several features for managing your deployments. For more information, see "Deploying with GitHub Actions."

Press alt+up to activate
Help and support
title: Using environments for deployment
shortTitle: Use environments for deployment
intro: You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.
product: '{% data reusables.gated-features.environments %}'
redirect_from:
  - Request/actions/reference/environments/Repo Approval Request/actions/reference/environments/Repo Approval 
  - /actions/deployment/environments
  - /actions/deployment/using-environments-for-deployment
topics:
  - CD request for digital approval 
  - Deployment
versions:
  fpt: '*'name: Deployment

on: Harmony Blue Branding 
  push: for Request for digital declaration 
    branches: Request for digital approval 
      - main

jobs: Required pre-purchased credit before alternative legacy APIs data entry workload of Merger Workspace layout and services input repo
  deployment:Url location site code maintenance is an legal certification business platform creatively format work flow on Port editing site fix assistance solutions for this primary website to be able to make changes to historical data content could be beneficial or either dangerous for the our business digital system please take in this consideration before adding your input on this business development resources page Thank you 
    runs-on: editor are welcome upon administrative service approvals ubuntu-latest website work-flow information updates are greatly appreciated but please be aware and vigilant of your online asset protocols before editing this incorporation business online website pages
    Please include your the address to your objective and your own itemized information before completing your work flow with this apmis-step is needed first environment: what digital declaration to this incorporation digital declaration of operating add-on production
    steps: what work criteria are you adding to this incorporation digital account 
      - name: please add benefitical to the public library rescription proxy for the public url sites space include the proper connected dedications formation to be completed by you like updating adding on resources to provide help with affiliate management support/input to assistance with resources for references to help with errors or support products fixes/assistance solutions for support products with resources to nonprofit organization fundraising publication before completion or deploy
        # ...deployment-specific steps include the proper regards to the date, time and effort source link to company active legacy APIs Group url location library information for usage status on all the digital programs development and support products that are attached to the business website page and links to this incorporation affiliated workspace entity 
  ghes: '*'
  ghec: '*'
---


## About environments

Environments are used to describe a general deployment target like `production`, `staging`, or `development`. When a {% data variables.product.prodname_actions %} workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

{% ifversion actions-break-glass %}Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."{% endif %}

{% ifversion fpt %}
{% note %}

**Note:** Users with {% data variables.product.prodname_free_user %} plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %} can configure environments for private repositories. For more information, see "[AUTOTITLE](/get-started/learning-about-github/githubs-plans)."

{% endnote %}
{% endif %}

## Deployment protection rules

Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches.{% ifversion actions-custom-deployment-protection-rules-beta %} You can also create and implement custom protection rules powered by {% data variables.product.prodname_github_apps %} to use third-party systems to control deployments referencing environments configured on {% data variables.location.product_location %}.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

{% data reusables.actions.custom-deployment-protection-rules-limits %}

{% endif %}

### Required reviewers

Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

{% ifversion deployments-prevent-self-approval %}You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.{% endif %}

For more information on reviewing jobs that reference an environment with required reviewers, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments)."

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, required reviewers are only available for public repositories.

{% endnote %}{% endif %}

### Wait timer

Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, wait timers are only available for public repositories.

{% endnote %}{% endif %}

### Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}

Use deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} to restrict which branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to the environment. Below are the options for deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} for an environment:

{% ifversion deployment-protections-tag-patterns %}
- **No restriction**: No restriction on which branch or tag can deploy to the environment.
{%- else %}
- **All branches**: All branches in the repository can deploy to the environment.
{%- endif %}
- **Protected branches{% ifversion deployment-protections-tag-patterns %} only{% endif %}**: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "[AUTOTITLE](/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)."{% ifversion actions-protected-branches-restrictions %}

  {% note %}

  **Note:** Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

  {% endnote %}{% endif %}
- **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**: Only branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} that match your specified name patterns can deploy to the environment.

  If you specify `releases/*` as a deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule, only a branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} whose name begins with `releases/` can deploy to the environment. (Wildcard characters will not match `/`. To match branches{% ifversion deployment-protections-tag-patterns %} or tags{% endif %} that begin with `release/` and contain an additional single slash, use `release/*/*`.) If you add `main` as a branch rule, a branch named `main` can also deploy to the environment. For more information about syntax options for deployment branches, see the [Ruby `File.fnmatch` documentation](https://ruby-doc.org/core-2.5.1/File.html#method-c-fnmatch).

  {% ifversion deployment-protections-tag-patterns %}

  {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

  {% endif %}

{% ifversion fpt %}{% note %}

**Note:** Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are also available for private repositories.

{% endnote %}{% endif %}

{% ifversion actions-break-glass %}

### Allow administrators to bypass configured protection rules

By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

{% ifversion fpt %}{% note %}

**Note:** Allowing administrators to bypass protection rules is all available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}
{% endif %}

{% ifversion actions-custom-deployment-protection-rules-beta %}

### Custom deployment protection rules

{% data reusables.actions.custom-deployment-protection-rules-beta-note %}

{% data reusables.actions.about-custom-deployment-protection-rules %} For more information, see "[AUTOTITLE](/actions/deployment/protecting-deployments/creating-custom-deployment-protection-rules)".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "[AUTOTITLE](/actions/deployment/protecting-deployments/configuring-custom-deployment-protection-rules)."

{% ifversion fpt %}{% note %}

**Note:** Custom deployment protection rules are only available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}

{% endif %}

## Environment secrets

Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "[AUTOTITLE](/actions/security-guides/using-secrets-in-github-actions)."

{% ifversion fpt %}
{% note %}

**Notes:**

- Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."
- Environment secrets are only available in public repositories if you are using {% data variables.product.prodname_free_user %}. For access to environment secrets in private or internal repositories, you must use {% data variables.product.prodname_pro %}, {% data variables.product.prodname_team %}, or {% data variables.product.prodname_enterprise %}. For more information on switching your plan, see "[AUTOTITLE](/billing/managing-the-plan-for-your-github-account/upgrading-your-accounts-plan)."

{% endnote %}
{% else %}
{% note %}

**Note:** Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."

{% endnote %}
{% endif %}

## Environment variables

Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[AUTOTITLE](/actions/learn-github-actions/variables)."

{% ifversion fpt %}{% note %}

**Note:** Environment variables are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, environment variables are also available for private repositories.

{% endnote %}{% endif %}

## Creating an environment

{% data reusables.actions.permissions-statement-environment %}

{% ifversion fpt %}
{% note %}

**Notes:**

- Creation of an environment in a private repository is available to organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %}.
- Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.

{% endnote %}
{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
{% data reusables.actions.new-environment %}
{% data reusables.actions.name-environment %}
1. Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "[Required reviewers](#required-reviewers)."
   1. Select **Required reviewers**.
   1. Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
   {% ifversion deployments-prevent-self-approval %}1. Optionally, to prevent users from approving workflows runs that they triggered, select **Prevent self-review**.{% endif %}
   1. Click **Save protection rules**.
1. Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "[Wait timer](#wait-timer)."
   1. Select **Wait timer**.
   1. Enter the number of minutes to wait.
   1. Click **Save protection rules**.
{%- ifversion actions-break-glass %}
1. Optionally, disallow bypassing configured protection rules. For more information, see "[Allow administrators to bypass configured protection rules](#allow-administrators-to-bypass-configured-protection-rules)."
   1. Deselect **Allow administrators to bypass configured protection rules**.
   1. Click **Save protection rules**.
{%- endif %}
{%- ifversion actions-custom-deployment-protection-rules-beta %}
1. Optionally, enable any custom deployment protection rules that have been created with {% data variables.product.prodname_github_apps %}. For more information, see "[Custom deployment protection rules](#custom-deployment-protection-rules)."
   1. Select the custom protection rule you want to enable.
   1. Click **Save protection rules**.
{%- endif %}
1. Optionally, specify what branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to this environment. For more information, see "[Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}](/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches{% ifversion deployment-protections-tag-patterns %}-and-tags{% endif %})."
   1. Select the desired option in the **Deployment branches** dropdown.
   1. If you chose **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**, to add a new rule, click **Add deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule**
   {% ifversion deployment-protections-tag-patterns %}1. In the "Ref type" dropdown menu, depending on what rule you want to apply, click **{% octicon "git-branch" aria-label="The branch icon" %} Branch** or **{% octicon "tag" aria-label="The tag icon" %} Tag**.{% endif %}
   1. Enter the name pattern for the branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} that you want to allow.
      {% ifversion deployment-protections-tag-patterns %}

      {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

      {% endif %}
   1. Click **Add rule**.
1. Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "[Environment secrets](#environment-secrets)."
   1. Under **Environment secrets**, click **Add Secret**.
   1. Enter the secret name.
   1. Enter the secret value.
   1. Click **Add secret**.
1. Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[Environment variables](#environment-variables)."
   1. Under **Environment variables**, click **Add Variable**.
   1. Enter the variable name.
   1. Enter the variable value.
   1. Click **Add variable**.

You can also create and configure environments through the REST API. For more information, see "[AUTOTITLE](/rest/deployments/environments)," "[AUTOTITLE](/rest/actions/secrets)," "[AUTOTITLE](/rest/actions/variables)," and "[AUTOTITLE](/rest/deployments/branch-policies)."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

## Using an environment

Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

{% data reusables.actions.environment-example %}

## Deleting an environment

{% data reusables.actions.permissions-statement-environment %}

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
1. Next to the environment that you want to delete, click {% octicon "trash" aria-label="Delete environment" %}.
1. Click **I understand, delete this environment**.

You can also delete environments through the REST API. For more information, see "[AUTOTITLE](/rest/repos#environments)."

## How environments relate to deployments

{% data reusables.actions.environment-deployment-event %}

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "[AUTOTITLE](/rest/repos#deployments)," "[AUTOTITLE](/graphql/reference/objects#deployment)" (GraphQL API), or "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads#deployment)."

## Next steps

{% data variables.product.prodname_actions %} provides several features for managing your deployments. For more information, see "[AUTOTITLE](/actions/deployment/about-deployments/deploying-with-github-actions)."
Thank you for sharing your online asset with our team workspace for our nonprofit organization website page and site affiliate stems online business sites programs development director layouts for our business website url https and http ID multiple level digital site domain web portal database access point by Google Domain Sites Enterprise of view interface proxy server legacy with Google Business Manager Suite and Google Global maps Proxy Home and special Mediant extinct format digital partnership, Affiliate Marketing applications and services branding promotional content for the public network and multiple companies digital YouTube,Need for Good, Black American Computer Science & Technology analysts Engineering services Beta Developer Business AI Executive AI Executive director layouts and education services for Technology analysts Program Manager Association Manager Advanced marketing services Agency Al Gemini digital legion solutions services for community console and accreditions-campaign services Elite's Expertise Technology analysis data documention official prospects associated registered business platform creatively forum connected with targeted data entry editor for productions to industry support for Google Advertisement Company active legacy APIs Group for Google Global communications projects formation for assessment materials and services grant program engineering youth education projects with resources relations Google database coding Expert and Impact Internal Federal Online Safety forums Interpol EU digital data protections business online affiliate, US Government Historianal Engineer/ GU federal credit historianal engineer authorizations data ancestor publish media operational status Creative Workshop Research Educator/publications of federal laws and development director of digital declaration of operating title developer/media reporter freelancer/Digital media layouts Editors library of admissions resources to document CFR social auditor/Legal Congressional library NIST- Publication editor/author network stems/IRS legacy API proper connected to published work flow data for documentions and publication governmental education business programs/OSHA office operations education business services operations legal program Manager Education business website affiliate marketing branding institutionalized workshop Administrative role in ID access member and ID me digital programs affiliate online network Digital primary contact of admin Publication authorized to provide assistance assessment and support products editor enforcement to provisions of support and services for execution to directory layouts and quote information for publication listings optional-way for educational solutions for business accounts for IDme.gov/Congressional-library .gov and digital CFR.gov publication listings for documention data entry workload and digital support input for provisions forms for Google data questionnaire and email assistance logistics and 
partnership with logistics taskforce advocate & member of investors to investigation on edition Mediant extinct format diverse director and discussion regarding console quote for various resoureful commissions sites business resolution to public website user, business organization and individual requested assistance with resources and information for usage status on legacy data updates forms on the public website regarding review & question for answers to solutions topics for website help pages with support assistant requests. Our business website page and online business are innovative and creatively forum connected to assistance individuals with what ever they may need help with bith on and off line as well as an active member of multiple interactions Digital Health and assistance works for our assistance with the Google brand name for our own itemized form of public assistance and services for this primary caring organization that's fully invested in various types of business affiliated networks/programs / partner/ branding promotional content digital declaration social media creations/affiliate online business marketing/education services and supporting editing skills to learn customized and data entry for different types of community supportive systems tools-Productions-Make alliances all-types of business associate with resources support for any platform or operator division eaudience in input assessment solutions Creative workshop business services sales consultant in the constructively directed streams of public information to help with the the security data and judgement and support assistance with online knowledge of maintaining the proper connected dedications formation for everyone to digital obtain the sublevels of information and services understand and direction and communication accredited knowledge and circumstances educational solutions for their requests to be addressed in a respectful manner and equally with the proper connected information to the public resources and architecture inquire if they're needed understanding of accessibility to reflect, adamantly fix the problem of what ever the quarry was in the constructively directed streams of course offer to exactly direct interaction with the proper instructions for their requests based upon the proper dedications formation reference to the public information to be administrator on behalf of the digital business level particulars while additional access to update internal requirements details assignment direct deposit install instructions for the public data documention and the entitled format of the inquiry output for the publicated directory services methods to provide help with assurance of whatever corporate clarification to be used for the reasonable assetment of input that's would be needed for that individualized record of requestsending unusual form for the most recent/accord information to be delivered for that precise and targeted questions to the proper connected provisions with in the constructively correctiodesigned material for asset management of misrepresented overall consistency reputation for the companies integrity, safety, and legitimate policy's and general guidelines of the digital business online regulated language and procalls to be upheld in regards to their own requirements for this online business website page digital declaration invitation for the next step of course the addressing the proper infornative about input to available to choose the most effective beneficently revision term of agency aggressive interest in obtaining the best opportunity for investing the public with the multi factor investing knowledge for the duty infinite information and services site fix with resources to two or more option available to choose from this position offers the customer to be able to create an secondary opinion on different methods to try to be able to gain control and access on trying to accord complete whatever issues that maybe an issued response to the file transfer resources to provide help with the errors progressive data entry inquiries to this documented incident report for the unlimited resources to information rely required to commotions accreditions-campaign really reputable to the public question for assistance with resources by the intended source link and data accommodations for the proposal application services trouble shooting resolved encounter with the reliable programs data connection with donations inquiry to be answered with the device instead step in calculated reliability means for the solutions to the topic of candidates console services unconditionally contact of interest of content containing solutions that are attached to and relevant to the question of the topic of each individual request for digital assistance with resources requests for the most recent a certain quality dedicated rebutionals to production and services includion data recovery information to deverepeate/offer platforms advice for the total reference to help execute and supportive measures for optimum instruction of accurate understanding step by step with the instructions in language that innovatively understandable to the average individual's for the results information layouts in a full clear and content to betterment that most people can combative comprehend the digital resources instructive formation method for everyday adults with little of no interest in learning computer laggo or logistics writings for comparison of repost ID reactivate compatible computer science technology position instructions vs regular function format language detail on how to use or fix errors and computer updates for delivery reposition to accommodate a really teachable upstanding to help with the desired format to offer fully data and easily flow written individuals information for a more reliable user experience with resources and wanted to share interest succession of gainful interactions, and more direct hand on usages for that and more data products than can be produced and purchased by the intended cusmoter for the extra revolution for the public health od public perception of this primary company active in business sales and longevity status with the proper connected dedications for the understanding of accessibility to reflect materials that the average person can understand and use affirmatively daily or either assessment of assets to provide assistance with needed resources for the addition public prosecutor to be conserved effect on efforts to make life decisions for a easier relationship with building the public knowledge of useful content and self services understanding of accessibility to reflect on the values of that rebrand realistic resolution for the proper connected educations formation of the digital material and essential facts about self service support for future reference to help with error fixes that gainful profitable meaningful Internally investment in regards to resourcful methods to provide assistance with the proper easy understand reasonable assets for the self confidence in be able to make adjustments for the individualize ability to provide self Care Provider for ones self by providing the proper multipurpose operational services steps in regards to this one 
request for digital programs assistance with the device and digital programs products that are directional compatible with the proper steps by step instructions and in the information landscape of language that's clear and precise to accurately address concerns the proper regards request of assistance with or either about a products or services request for digital programs development director layouts and education for instructions on how to officially operate the proper the digital products that maybe needed at that point for resources and details instructions assessments issues. This organization has always had the best and most meaningful interest in privileged in regards to the public delivery of service data accessment in regards to assisting individuals with the proper connected educations formation for individuals to become properly informated with the necessties tools, data, institutional related instructions, transactional status recording details, trouble shooting methods to respectfully and written in regards to underable language steps to the provider proper understandable response to the public logistics of the contractly regarding methods of direct data recovery and usage instructions for with an update most openings for incorprative formcreatively directoryhandout for director full compatible comprehension for required edition information to upon the public requested for real situation inquiry to provision's of addressing the proper dedications formation for the issues of connection questions and answers of assistance with resources for references needed to be completed by development director assistance developers for Google blogs and community services representative that are available to help with any type of additionalitemized litemized for asset management of material on the public website that are available to help with individuals related in the constructively directed work flow assessment of the digital produces and services protocols for assistance with resources to provisions of items problems with the proper connected safety and policies for associated historically corporate clarification policies and rules of the digital platform for the users regulated language in the constructively correctiodesigned material form of with the decorative of the digital business online website and developer freelance platforms regulated language and data support systems tools-Productions-Make alliances all-types of concerns to the business request formats and editing delivery design database affidavit admissions to the primary care providers mastering estimated assistance with the device for the time of the demographic that's related to the public register for handling the digital programs development editor and educational solutions workspaces/entitled digital programs fields of admissions to this incorporation affiliated workspace title of the digital coding and encompass business programmer management reserve required networks dedications of reasonables /market place in regards to Google platform creative Merchant  online business sites Collaboration Account Manager workshop Administrative connected with targeted Digital Primary programmers scholarship to provisions by Microsoft Data Business Relations Marketing LLC online business sites space for Entity and financial service memberships. Dedications of admissions to Harmony Blue Branding Organization Business Group for Google industries & affiliate online services. This organization assistment digital declaration communities in the constructively directed proper composition of the digital business online website work-flow information.The professionalism was directory targeted focused groups. Aligned for our relatives platform and the whole founded objective as an financial needed for govnen revenue goal for Supportive interest and equipment services for the needed resources and services fees goal of the digital business financially funding ongoing income for the public emerged infrastructure security both digitally and pumpkin to be provided effective benefits of ensured for the public sector that every single young individuals has been prevented with the proper connected dedications formation for the entire consult of the digital programs development and educational services, data access to cover the wireless work location, the proper number of devices and equipment for business services and supporting resourceful supplies for operations legal rights for the next few months of digital Business of more digital devices ti the property location and service representative office operations manager to help with growth and support several types of resoureful products and digital products software for future contact servers modeled to provide assistance with the best opportunity for the young individuals to succeed and mobility follow through the internal templates of information without being offset with digital logically issues, digital proforms database speed for a crosspoint platform router with resources to support a of 20 services device service all as once.The desire digital devices that have an operational status title to fighting history of becoming jetlack and development issues with the performance connection issues of jinky data access on site uploads to slow down after an few weeks even with resources to provide assistance for consideration time duration to resl time to automatic logout reload to the home website work-flow format even with the proper connected dedications formation of self proved to model to reset  focused hardware for the public safety of privacy data properly connected with the best suitable amount of firewall services equipment. poorly performing computers and devices still are known to becoming problematic with an short span of time with the daily  used forum and including the recognized digital entry of technology projects and designs for anyone who is using the property directly from the tribe source related fields of unlimited uploading and downloading materials that are associated with the device drive root redirect to the function settings and this is so even with resources of saving the proper connected resources to an extent thumb drive as well. I've been extremely fortunate to receive offers for products at a discount business price but the conditions based on the price of the bulk product did not meet and will not meet my expectations including them being refurbished it's also additional Factor business conditions of course of this months of this primary needed contributions and resources entity properties for development director layouts and education services for the public youth program reliability means for the results of the same the primary consideration of the digital business level of connection functionality of the digital business online demand security equipmentinterest in provisions of comparisons services for the public online business official prospects production of the Harmony Blue Branding LLC online business sites space in the incorporate status of the Publication records accredited nonprofit organization for the Harmony Blue, the digital business connection to synced detail to corporate clarification to the public forum for the Harmony Blue Foundation and unique classification for the Harmony House Foundation assets provide assistance with resources of additional details reference to reflect the public executive operations legal certification federal credit union of the Harmony Blue index as an multiple increments of the digital business online website work-flow merger page for qualified directly exempt status with the proper dedications standards of admissions resources this nonprofit organization legal verification records forms on network with the Secretary of state certified business office operations manager Owner Founder Cheyanna Henry and solely partnership with public records and license documentions records for sole business claiming to and for legalize owner of public reconciliation in the official prospects of associated registered business with both federal and state judication for legal administrative corporate direct access deposit data approval for operations status with the United States registered for legal operations for business services and operations services of corporate clarification business government office for registration regulations required to commotions accreditions-campaign with the BBB and certification business verification of public records by Yelp and Google Business operations with good legal rights to active declaration of operating title of Administrator grade A+ standings. Legacy financial ownership over all of alliance titles that are attached to the public business services operations legal rights to law federal government services based upon regulations required the proper connected position with resources and public data access records for linked to the business process records forms on relisted and financial filing requirements needed for this primary position to be with in the constructively directed format of the digital business online state of justify jurisdiction records forms on iexceptions provisions with no notarized expiration date of service for social revision to be ablity or applied for this primary business organization and business design database Group for prevention of the digital programs and architecture health to services associated registered to and publication owner transfer design  trademarked to be delivered in relation to this corporative and incorporation business organization services.All services supportive and professional submission for the publication inquiries for and about this and any other details information on operational status and digital declaration library of services.Are on my website work-flow merger page for qualified directly to the public records forms on networking and content communication accredited to this incorporation affiliated workspace title Account Manager and services for the public enlightened to this incorporation digital programs and development projects, foundation services and supporting resoureful health services and operations management community project design database information for usage status on legacy APIs Group chat or organization fundraising publication of the digital business online information for services. Reminder to be available for future reference to this incorporation business online information would be dedications and migration file folders to the url website work-flow merger pages of this primary position in caring with resources to nonprofit organization for the public view of the digital progressive information to be attached to the public historical website work-flow dashboard proxy server Domain and listed as correspondent for the public records forms legacy knowledge page for qualified directly on the values proper connected to the public dedications formation for the online businesses.All legal certification for the public libraries of interest in various types of products and digital services for the business response to this incorporation affiliated workspace title are on the public library url website website pages for this proxy servers. Observation about the benefitical of the digital business online website work-flow and business organization services mergers to the business development resources to nonprofit organization for the public development resources to provide assistance with the public this encoded within this website sites drive folders and digital historianal engineer authorizations in regards to declaration date and data entries to provided effective benefits are in the work spaces for this primary website primer proxy server legacy APIs website. All division of course audience of all three years of services are incorporated into the digital programs website digital pages of this primary URL multi-level link for this online campaign analytical publications as high 87% know legally operations of course of 2 or more years of services and operations legal rights to owner of office annual budgets for this business and finance services related information for the public resources records reflect materials that are attached and completed recorded for the business cab be found on this business data digital proxy server legacy APIs https openings and http openings and the whole website platform workspaces for historical content for visual recognition of the public library of admissions to various types of resoureful community projections to campaign analytical virtual public market contact in regards to revenue agency Dedications of admissions to various project design database information for this primary program to the business operations of publication of services and supporting resoureful health and youth community project and community effort to assistance with the proper dedications formation of the financial support and diversity development director layouts and education services for the public youth community services projects analytics promotional content ideas on custom crafts resources to nonprofit organization and business management program engineering department promoter and the whole website in traditional company active legacy APIs program engineering services for claiming methods to respectfully writing of during of works for grant writing projections for grant funding proposal abd application listed dayes of applied for real literacy commissions prospects production of abilities for achievvmultiple documents grant funding for services assistant director resources to help with nonprofit organization needs and resources requirements for professional provisions items for comparison of quality assistance to thrive property management preparation for proposals require to apply for existing administrators economical successfully following platform creatively forum connected synced targeted Digital Health assistant associate with resources to nonprofit organization and business foundation services for the public needs to accommodate the proper connected resources to provide help with growth of the companies abilities for the original particulars network online goals in achieved affection for the fundraising publication financial support of expansion efforts for resources to help more individuals, the proper functions and digital support for future devices and services equipment needed to be able to meet the public needs for access to relevant access to multiple dynamic digital programs and urgent support products to the web portal database information for usage of official prospects in regards to the business response for the children of the local communities for the device assistance with optional costs for the equipment and office operations supplies and offices functional company desktop office operations legal solutions products including the publication business services office furnitures for the proper connected dedications formaled item's request to provide the proper access to the business network digital programs for the public education programs development director of digital programs and support development resources for the proper dedicated hopeful abilities for achievements are the best opportunity to gainful commending for our youths and family services programs. Due to si many unrealistic situations in and with this apmis-step on this business data access to existing administrator assistance with the proper regards to the prixy servers legacy APIs access point of services and operations output linked database for the business page for this primary website always degraging to regularly payloads server Domain IP links to the business director digital business online website connected repetatively resource communication projections to request fix fork and code maintenance for the public url lodge on the public library database information to register domain sites and hist submit support site location for this online business to be always conversing to having static data issues daily with without any type of additionalitemized litemized reasons for this online related to repeatly be happening so frequently between every 6 to 9 months reaction time during the time of the very needed resources submission to application process for financial assistance request for business services operations resources funding grant submittions for the business needs of assistance with resources to the non-profit organization primary assistance with resources to help with principles projects.Digital issues surrounding this primary problem resourced in the self eduational knowledge of the digital respectful needs to be ables to essential expert knowledge and analysis skills to learn every single team member learning how to prioritize the proper digital programs development education business services operations specialist and sales Special edition for the public sector in regards to conpter science technology engineering and engineering logistics software management skills for the custom crafted resources to assist with the proper dedications provisions of advanced releases to products exist respect for developmentprerelease beta affiliate corporate sponsorship executive director layouts and education business management support systems tools-Productions-Make alliances openings for branding commission analysis company market for exterior demolition of primary test kit before release date of service software and Digital website work-flow connection with top category criteria corporation with in the technology fiejds and the top confirmed development software and firmware company market logistics testing performance pre-release for prompt entity properties trailer to use for limited capacity for testing of their system products before the publication launch date. I have the best experience with the most of thud typr of offers to from this organization collected collaborative branding promotional content program edition and merging with the faculty team contributions accessibly of the digital programs and signature corporations like IBM, Digital icon Douglas navg. software for mathematics and digital device technology and development, software engineer, authorizations data T-Mobile Wireless  Opera, Microsoft Data content Business workshop Administrative corporate sponsorship program for various resources and grant business website resources, Google docs data access to studio sponsorship for YouTube creative publication contract for Google industries,etc...the list extinct is too long to be able to review alliance various types of products affiliated with the proper regards to special digital distribution for the best opportunity for this online business website services to required online business sites and data access for the program execution to directory layouts itemized formal exceptions provisions items as resources network logistics to requested resources to nonprofit organization and business organization services development teams to provide assistance with the unlimited and excellent customer relations professionals dream of being able to achieve the vision obtained operations over the course of this business operations legacy of business associate director of education services for the public education business management relationships in regards to to active Dedications leadership and learning resources for expanding knowledge of maintaining itemized formal exceptions provisions of comparisons services representative of tenant executive director digital programs development education skills essential on the public value existing administrators economically successfully as well the proper related features are needed for the digital programs and achievements directly to depositing input and output data website work-flow information for future configuration and executive director design database coding Expert services for claiming methods engineering reliable programs safe for folk and report various types of rep repositories of data matrix and external factors platforms of operational status on puls format director layouts in Editorial direct access to multiple resources to reverse and revise coding error for fix assistance solutions about benefitical test switch to provide help with sight solutions of innovation directory layouts itemized reference format dymierrors category individualized recording on the values proper connected dedications formation for the public estimated assistance in the constructively streams in regards to the business response to the plugs and the drive in public library of course the title verify auto correct information for usage status on the public health administration services request for digital programs development assistant Lead to promotion self support products for seeking various edit ms data portal program engineering and workload source link acodes for the necessary details tgat ate needed to accommodate the digital coding trouble shooting content for overall consistency ratio of an authorized execution of yermsk pass through documents shell and the whole needs website fault resources runs to provide the proper connected plug ti to be attached to the public library or file information for usage status correct designed to provide direct dig requirements to the intended rebutionals errors shorten and runs to the pull test for the next debug submit to the input genuine supportive code sites link support products for the next and steps in regards to services true and finally pass transportation to the point of request for digital programs come gainful profitable meaningful Internally to be effective to help with support products for web portal Repo in regards to the website resource link to seek for historical reviews real time health to this work flow data documention and refigured changes to the outline output of the digital programs development of the foundation of relationship knowledge of the digital programs and availability for obtaining additional information and educational services in with the current specialisted field of study in computer science and digital science technology analysis of to respectfully accomplish the proper regards to complete call my ab respectable engineer and at least obtain the ultra development skills for custom crafts and forever evolving tr trending fields in regards to digital programs and safety data analytics experience with the proper regards to tine and structure study in the duration or probably 3more to 5 more years plus recent My authorizations data certificate for the relation to the understanding of every single available resources learning programs and educational skills for custom crafts incentives of one day becoming a real estate agent with the proper dedications certifications and licences  required to be completely honored to be considered as an experienced digital engineering and professional programs development in software design and support business management skill publication connected to the website renewal portal workspace title database coding effectively meet the recognitions requirements needed to obtain the ultra high quality dedicated for everyone to be able to obtain.The  team and me the principal and somewhat their platform teacher and motivation support for them to learn and own the ability to build up the proper infrastructure understanding with the best opportunity to reach this reliability programs goal needed to be conserved effective for asset with resources and data entry Firm in the constructively directed streams of capable abilities to provide assistance with the proper environment to assist with financial support, education editing writing experience with digital programs networks for social media marketing executive operations legion solutions services technology project design database coding system with development publication listings recognized skills set based upon the public team learning group editing videos and exercises for this primary position to hopefully get better and understanding of accessibility to reflect material for various types of business administration resources to nonprofit organization and business organization services assistance representative of corporate operations manager to help access with candidates to acknowledge that required pre-purchased studies experience to lecture with the device tools with Microsoft Data content forms to create an connection updated skills to custom crafts and received the proper dedications formation for obtaining quality team contributions accessibly in every single person ability to test for the public titles and recipient joint submission application for the next step in service requirements for this primary term in regards to owner coverage for the publication testing process for recipient estimated purchasing for each team member to be able to obtain their own personal licenses for the required field and experiences of study as listings for receiving the public records forms for benefitical certification business license titled of course studies course associated registered business Digital computer science technology development of corporate clarification professional provisions for themselves and executive directly to their own personal employment resume for the achievement official prospective titled associated registered business services operations The dedication of value in legal rights to be able to gain the proper certification in business and finance services sales Special Mediant list form of certified title, professional associated degree in business management of legal data recording, direct access to multiple resources online to verification of products regulations corporate network registration for audit digital programs development in virtual license business services operations and tax experience finance soes list extinct with the IRS letter offical approval for this online legacy of business associate course. For nonprofit organization members that are associated with this organization works and daily practices.This programs for architecture require to the public as an optional need education business to help with growth and development succeed offer succession of gainful estimated knowledge for the online class entrepreneurs and certification tools for tax accountant in legal rights to business workers in operational statues of becoming a real licensed tax advocate professional associated with the United States supportive assistance with resources to help with education business management of the operator of the foundation financial advisor and resources to provide internal revenue agency assistance with an internal security data filing taxes for the public citizens of America and the tax paying company that are in need of help with processing and preparing tax records online. When the public and legal forms position to receive connected commentary compensation for office operations legal filing assistance with resources for references. All services provider for the proper connected costs for helping team members the digital testing fees. This organization has been through so many different types of extremely struggling and economic setbacks and imagined data access challenging for this company first 2 years of services. As an understanding and relevant development heartbreaking situations for the public sector in the past and a bunch of other side issues that are presented to the business still to this date. We're outstandingly advocating for and on behalf of anyone who has been extremely dedicated to help with the volunteer services and operations support for the public community services payment from the public are just a well acknowledged Thank you for being their for the connection hands on work and within the public district ministry works with us and in commitment and time to help with making successful decisions on small community services projects and assisting us by recite the public community about our relationship knowledge and communication skills for creating ideas for public relations to help with touching the proper people who have and have been in or are actively going through homeless and other financial difficulties or essential facts experts dur to health care and documented psychological issues. Our primary organization services targeted construction missionaries of services and extremely dire  awareness custom growth downtown areas changes to the public library of admissions resources to educational solutions adviser and services graduated in the constructively corrections designed course materials that related to passed the digital certificate testing.This online experiences with the trust federal revenue agencies in the US educational programs, and online curriculums to the  educational solutions for this one or two listed website public courses. 
e public courses. 
---deployment target like production, staging, or development. When a GitHub ActionsGitHub Actions/Deployment/Target different environments/Use environments for deployment
Using environments for deployment
You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.

Who can use this feature?
Environments, environment secrets, and deployment protection rules are available in public repositories for all current GitHub plans. They are not available on legacy plans, such as Bronze, Silver, or Gold. For access to environments, environment secrets, and deployment branches in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, other deployment protection rules, such as a wait timer or required reviewers, are only available for public repositories.

In this article
About environments
Deployment protection rules
Environment secrets
Environment variables
Creating an environment
Using an environment
Deleting an environment
How environments relate to deployments
Next steps
About environments
Environments are used to describe a general deployment target like production, staging, or development. When a GitHub Actions workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "Viewing deployment history."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "Reviewing deployments."

Note: Users with GitHub Free plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with GitHub Team and users with GitHub Pro can configure environments for private repositories. For more information, see "GitHub’s plans."

Deployment protection rules
Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches. You can also create and implement custom protection rules powered by GitHub Apps to use third-party systems to control deployments referencing environments configured on GitHub.com.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

Note: Any number of GitHub Apps-based deployment protection rules can be installed on a repository. However, a maximum of 6 deployment protection rules can be enabled on any environment at the same time.

Required reviewers
Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.

For more information on reviewing jobs that reference an environment with required reviewers, see "Reviewing deployments."

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, required reviewers are only available for public repositories.

Wait timer
Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, wait timers are only available for public repositories.

Deployment branches and tags
Use deployment branches and tags to restrict which branches and tags can deploy to the environment. Below are the options for deployment branches and tags for an environment:

No restriction: No restriction on which branch or tag can deploy to the environment.

Protected branches only: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "About protected branches."

Note: Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

Selected branches and tags: Only branches and tags that match your specified name patterns can deploy to the environment.

If you specify releases/* as a deployment branch or tag rule, only a branch or tag whose name begins with releases/ can deploy to the environment. (Wildcard characters will not match /. To match branches or tags that begin with release/ and contain an additional single slash, use release/*/*.) If you add main as a branch rule, a branch named main can also deploy to the environment. For more information about syntax options for deployment branches, see the Ruby File.fnmatch documentation.

Note: Name patterns must be configured for branches or tags individually.

Note: Deployment branches and tags are available for all public repositories. For users on GitHub Pro or GitHub Team plans, deployment branches and tags are also available for private repositories.

Allow administrators to bypass configured protection rules
By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "Reviewing deployments."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

Note: Allowing administrators to bypass protection rules is only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Custom deployment protection rules
Note: Custom deployment protection rules are currently in public beta and subject to change.

You can enable your own custom protection rules to gate deployments with third-party services. For example, you can use services such as Datadog, Honeycomb, and ServiceNow to provide automated approvals for deployments to GitHub.com. For more information, see "Creating custom deployment protection rules".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "Configuring custom deployment protection rules."

Note: Custom deployment protection rules are only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Environment secrets
Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "Using secrets in GitHub Actions."

Notes:

Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "Security hardening for GitHub Actions."
Environment secrets are only available in public repositories if you are using GitHub Free. For access to environment secrets in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. For more information on switching your plan, see "Upgrading your account's plan."
Environment variables
Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the vars context. For more information, see "Variables."

Note: Environment variables are available for all public repositories. For users on GitHub Pro or GitHub Team plans, environment variables are also available for private repositories.

Creating an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Notes:

Creation of an environment in a private repository is available to organizations with GitHub Team and users with GitHub Pro.
Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.
On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Click New environment.

Enter a name for the environment, then click Configure environment. Environment names are not case sensitive. An environment name may not exceed 255 characters and must be unique within the repository.

Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "Required reviewers."

Select Required reviewers.
Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
Optionally, to prevent users from approving workflows runs that they triggered, select Prevent self-review.
Click Save protection rules.
Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "Wait timer."

Select Wait timer.
Enter the number of minutes to wait.
Click Save protection rules.
Optionally, disallow bypassing configured protection rules. For more information, see "Allow administrators to bypass configured protection rules."

Deselect Allow administrators to bypass configured protection rules.
Click Save protection rules.
Optionally, enable any custom deployment protection rules that have been created with GitHub Apps. For more information, see "Custom deployment protection rules."

Select the custom protection rule you want to enable.
Click Save protection rules.
Optionally, specify what branches and tags can deploy to this environment. For more information, see "Deployment branches and tags."

Select the desired option in the Deployment branches dropdown.

If you chose Selected branches and tags, to add a new rule, click Add deployment branch or tag rule

In the "Ref type" dropdown menu, depending on what rule you want to apply, click  Branch or  Tag.

Enter the name pattern for the branch or tag that you want to allow.

Note: Name patterns must be configured for branches or tags individually.

Click Add rule.

Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "Environment secrets."

Under Environment secrets, click Add Secret.
Enter the secret name.
Enter the secret value.
Click Add secret.
Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the vars context. For more information, see "Environment variables."

Under Environment variables, click Add Variable.
Enter the variable name.
Enter the variable value.
Click Add variable.
You can also create and configure environments through the REST API. For more information, see "REST API endpoints for deployment environments," "REST API endpoints for GitHub Actions Secrets," "REST API endpoints for GitHub Actions variables," and "REST API endpoints for deployment branch policies."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

Using an environment
Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "Viewing deployment history."

You can specify an environment for each job in your workflow. To do so, add a jobs.<job_id>.environment key followed by the name of the environment.

For example, this workflow will use an environment called production.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: deploy
        # ...deployment-specific steps
When the above workflow runs, the deployment job will be subject to any rules configured for the production environment. For example, if the environment requires reviewers, the job will pause until one of the reviewers approves the job.

You can also specify a URL for the environment. The specified URL will appear on the deployments page for the repository (accessed by clicking Environments on the home page of your repository) and in the visualization graph for the workflow run. If a pull request triggered the workflow, the URL is also displayed as a View deployment button in the pull request timeline.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: 
      name: production
      url: https://github.com
    steps:
      - name: deploy
        # ...deployment-specific steps
Deleting an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Next to the environment that you want to delete, click .

Click I understand, delete this environment.

You can also delete environments through the REST API. For more information, see "REST API endpoints for repositories."

How environments relate to deployments
When a workflow job that references an environment runs, it creates a deployment object with the environment property set to the name of your environment. As the workflow progresses, it also creates deployment status objects with the environment property set to the name of your environment, the environment_url property set to the URL for environment (if specified in the workflow), and the state property set to the status of the job.

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "REST API endpoints for repositories," "Objects" (GraphQL API), or "Webhook events and payloads."

Next steps
GitHub Actions provides several features for managing your deployments. For more information, see "Deploying with GitHub Actions."

Press alt+up to activate
Help and support
title: Using environments for deployment
shortTitle: Use environments for deployment
intro: You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.
product: '{% data reusables.gated-features.environments %}'
redirect_from:
  - Request/actions/reference/environments/Repo Approval Request/actions/reference/environments/Repo Approval 
  - /actions/deployment/environments
  - /actions/deployment/using-environments-for-deployment
topics:
  - CD request for digital approval 
  - Deployment
versions:
  fpt: '*'name: Deployment

on: Harmony Blue Branding 
  push: for Request for digital declaration 
    branches: Request for digital approval 
      - main

jobs: Required pre-purchased credit before alternative legacy APIs data entry workload of Merger Workspace layout and services input repo
  deployment:Url location site code maintenance is an legal certification business platform creatively format work flow on Port editing site fix assistance solutions for this primary website to be able to make changes to historical data content could be beneficial or either dangerous for the our business digital system please take in this consideration before adding your input on this business development resources page Thank you 
    runs-on: editor are welcome upon administrative service approvals ubuntu-latest website work-flow information updates are greatly appreciated but please be aware and vigilant of your online asset protocols before editing this incorporation business online website pages
    Please include your the address to your objective and your own itemized information before completing your work flow with this apmis-step is needed first environment: what digital declaration to this incorporation digital declaration of operating add-on production
    steps: what work criteria are you adding to this incorporation digital account 
      - name: please add benefitical to the public library rescription proxy for the public url sites space include the proper connected dedications formation to be completed by you like updating adding on resources to provide help with affiliate management support/input to assistance with resources for references to help with errors or support products fixes/assistance solutions for support products with resources to nonprofit organization fundraising publication before completion or deploy
        # ...deployment-specific steps include the proper regards to the date, time and effort source link to company active legacy APIs Group url location library information for usage status on all the digital programs development and support products that are attached to the business website page and links to this incorporation affiliated workspace entity 
  ghes: '*'
  ghec: '*'
---


## About environments

Environments are used to describe a general deployment target like `production`, `staging`, or `development`. When a {% data variables.product.prodname_actions %} workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

{% ifversion actions-break-glass %}Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."{% endif %}

{% ifversion fpt %}
{% note %}

**Note:** Users with {% data variables.product.prodname_free_user %} plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %} can configure environments for private repositories. For more information, see "[AUTOTITLE](/get-started/learning-about-github/githubs-plans)."

{% endnote %}
{% endif %}

## Deployment protection rules

Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches.{% ifversion actions-custom-deployment-protection-rules-beta %} You can also create and implement custom protection rules powered by {% data variables.product.prodname_github_apps %} to use third-party systems to control deployments referencing environments configured on {% data variables.location.product_location %}.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

{% data reusables.actions.custom-deployment-protection-rules-limits %}

{% endif %}

### Required reviewers

Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

{% ifversion deployments-prevent-self-approval %}You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.{% endif %}

For more information on reviewing jobs that reference an environment with required reviewers, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments)."

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, required reviewers are only available for public repositories.

{% endnote %}{% endif %}

### Wait timer

Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, wait timers are only available for public repositories.

{% endnote %}{% endif %}

### Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}

Use deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} to restrict which branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to the environment. Below are the options for deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} for an environment:

{% ifversion deployment-protections-tag-patterns %}
- **No restriction**: No restriction on which branch or tag can deploy to the environment.
{%- else %}
- **All branches**: All branches in the repository can deploy to the environment.
{%- endif %}
- **Protected branches{% ifversion deployment-protections-tag-patterns %} only{% endif %}**: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "[AUTOTITLE](/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)."{% ifversion actions-protected-branches-restrictions %}

  {% note %}

  **Note:** Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

  {% endnote %}{% endif %}
- **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**: Only branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} that match your specified name patterns can deploy to the environment.

  If you specify `releases/*` as a deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule, only a branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} whose name begins with `releases/` can deploy to the environment. (Wildcard characters will not match `/`. To match branches{% ifversion deployment-protections-tag-patterns %} or tags{% endif %} that begin with `release/` and contain an additional single slash, use `release/*/*`.) If you add `main` as a branch rule, a branch named `main` can also deploy to the environment. For more information about syntax options for deployment branches, see the [Ruby `File.fnmatch` documentation](https://ruby-doc.org/core-2.5.1/File.html#method-c-fnmatch).

  {% ifversion deployment-protections-tag-patterns %}

  {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

  {% endif %}

{% ifversion fpt %}{% note %}

**Note:** Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are also available for private repositories.

{% endnote %}{% endif %}

{% ifversion actions-break-glass %}

### Allow administrators to bypass configured protection rules

By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

{% ifversion fpt %}{% note %}

**Note:** Allowing administrators to bypass protection rules is all available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}
{% endif %}

{% ifversion actions-custom-deployment-protection-rules-beta %}

### Custom deployment protection rules

{% data reusables.actions.custom-deployment-protection-rules-beta-note %}

{% data reusables.actions.about-custom-deployment-protection-rules %} For more information, see "[AUTOTITLE](/actions/deployment/protecting-deployments/creating-custom-deployment-protection-rules)".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "[AUTOTITLE](/actions/deployment/protecting-deployments/configuring-custom-deployment-protection-rules)."

{% ifversion fpt %}{% note %}

**Note:** Custom deployment protection rules are only available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}

{% endif %}

## Environment secrets

Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "[AUTOTITLE](/actions/security-guides/using-secrets-in-github-actions)."

{% ifversion fpt %}
{% note %}

**Notes:**

- Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."
- Environment secrets are only available in public repositories if you are using {% data variables.product.prodname_free_user %}. For access to environment secrets in private or internal repositories, you must use {% data variables.product.prodname_pro %}, {% data variables.product.prodname_team %}, or {% data variables.product.prodname_enterprise %}. For more information on switching your plan, see "[AUTOTITLE](/billing/managing-the-plan-for-your-github-account/upgrading-your-accounts-plan)."

{% endnote %}
{% else %}
{% note %}

**Note:** Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."

{% endnote %}
{% endif %}

## Environment variables

Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[AUTOTITLE](/actions/learn-github-actions/variables)."

{% ifversion fpt %}{% note %}

**Note:** Environment variables are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, environment variables are also available for private repositories.

{% endnote %}{% endif %}

## Creating an environment

{% data reusables.actions.permissions-statement-environment %}

{% ifversion fpt %}
{% note %}

**Notes:**

- Creation of an environment in a private repository is available to organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %}.
- Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.

{% endnote %}
{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
{% data reusables.actions.new-environment %}
{% data reusables.actions.name-environment %}
1. Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "[Required reviewers](#required-reviewers)."
   1. Select **Required reviewers**.
   1. Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
   {% ifversion deployments-prevent-self-approval %}1. Optionally, to prevent users from approving workflows runs that they triggered, select **Prevent self-review**.{% endif %}
   1. Click **Save protection rules**.
1. Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "[Wait timer](#wait-timer)."
   1. Select **Wait timer**.
   1. Enter the number of minutes to wait.
   1. Click **Save protection rules**.
{%- ifversion actions-break-glass %}
1. Optionally, disallow bypassing configured protection rules. For more information, see "[Allow administrators to bypass configured protection rules](#allow-administrators-to-bypass-configured-protection-rules)."
   1. Deselect **Allow administrators to bypass configured protection rules**.
   1. Click **Save protection rules**.
{%- endif %}
{%- ifversion actions-custom-deployment-protection-rules-beta %}
1. Optionally, enable any custom deployment protection rules that have been created with {% data variables.product.prodname_github_apps %}. For more information, see "[Custom deployment protection rules](#custom-deployment-protection-rules)."
   1. Select the custom protection rule you want to enable.
   1. Click **Save protection rules**.
{%- endif %}
1. Optionally, specify what branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to this environment. For more information, see "[Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}](/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches{% ifversion deployment-protections-tag-patterns %}-and-tags{% endif %})."
   1. Select the desired option in the **Deployment branches** dropdown.
   1. If you chose **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**, to add a new rule, click **Add deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule**
   {% ifversion deployment-protections-tag-patterns %}1. In the "Ref type" dropdown menu, depending on what rule you want to apply, click **{% octicon "git-branch" aria-label="The branch icon" %} Branch** or **{% octicon "tag" aria-label="The tag icon" %} Tag**.{% endif %}
   1. Enter the name pattern for the branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} that you want to allow.
      {% ifversion deployment-protections-tag-patterns %}

      {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

      {% endif %}
   1. Click **Add rule**.
1. Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "[Environment secrets](#environment-secrets)."
   1. Under **Environment secrets**, click **Add Secret**.
   1. Enter the secret name.
   1. Enter the secret value.
   1. Click **Add secret**.
1. Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[Environment variables](#environment-variables)."
   1. Under **Environment variables**, click **Add Variable**.
   1. Enter the variable name.
   1. Enter the variable value.
   1. Click **Add variable**.

You can also create and configure environments through the REST API. For more information, see "[AUTOTITLE](/rest/deployments/environments)," "[AUTOTITLE](/rest/actions/secrets)," "[AUTOTITLE](/rest/actions/variables)," and "[AUTOTITLE](/rest/deployments/branch-policies)."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

## Using an environment

Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

{% data reusables.actions.environment-example %}

## Deleting an environment

{% data reusables.actions.permissions-statement-environment %}

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
1. Next to the environment that you want to delete, click {% octicon "trash" aria-label="Delete environment" %}.
1. Click **I understand, delete this environment**.

You can also delete environments through the REST API. For more information, see "[AUTOTITLE](/rest/repos#environments)."

## How environments relate to deployments

{% data reusables.actions.environment-deployment-event %}

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "[AUTOTITLE](/rest/repos#deployments)," "[AUTOTITLE](/graphql/reference/objects#deployment)" (GraphQL API), or "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads#deployment)."

## Next steps

{% data variables.product.prodname_actions %} provides several features for managing your deployments. For more information, see "[AUTOTITLE](/actions/deployment/about-deployments/deploying-with-github-actions)."
Thank you for sharing your online asset with our team workspace for our nonprofit organization website page and site affiliate stems online business sites programs development director layouts for our business website url https and http ID multiple level digital site domain web portal database access point by Google Domain Sites Enterprise of view interface proxy server legacy with Google Business Manager Suite and Google Global maps Proxy Home and special Mediant extinct format digital partnership, Affiliate Marketing applications and services branding promotional content for the public network and multiple companies digital YouTube,Need for Good, Black American Computer Science & Technology analysts Engineering services Beta Developer Business AI Executive AI Executive director layouts and education services for Technology analysts Program Manager Association Manager Advanced marketing services Agency Al Gemini digital legion solutions services for community console and accreditions-campaign services Elite's Expertise Technology analysis data documention official prospects associated registered business platform creatively forum connected with targeted data entry editor for productions to industry support for Google Advertisement Company active legacy APIs Group for Google Global communications projects formation for assessment materials and services grant program engineering youth education projects with resources relations Google database coding Expert and Impact Internal Federal Online Safety forums Interpol EU digital data protections business online affiliate, US Government Historianal Engineer/ GU federal credit historianal engineer authorizations data ancestor publish media operational status Creative Workshop Research Educator/publications of federal laws and development director of digital declaration of operating title developer/media reporter freelancer/Digital media layouts Editors library of admissions resources to document CFR social auditor/Legal Congressional library NIST- Publication editor/author network stems/IRS legacy API proper connected to published work flow data for documentions and publication governmental education business programs/OSHA office operations education business services operations legal program Manager Education business website affiliate marketing branding institutionalized workshop Administrative role in ID access member and ID me digital programs affiliate online network Digital primary contact of admin Publication authorized to provide assistance assessment and support products editor enforcement to provisions of support and services for execution to directory layouts and quote information for publication listings optional-way for educational solutions for business accounts for IDme.gov/Congressional-library .gov and digital CFR.gov publication listings for documention data entry workload and digital support input for provisions forms for Google data questionnaire and email assistance logistics and 
partnership with logistics taskforce advocate & member of investors to investigation on edition Mediant extinct format diverse director and discussion regarding console quote for various resoureful commissions sites business resolution to public website user, business organization and individual requested assistance with resources and information for usage status on legacy data updates forms on the public website regarding review & question for answers to solutions topics for website help pages with support assistant requests. Our business website page and online business are innovative and creatively forum connected to assistance individuals with what ever they may need help with bith on and off line as well as an active member of multiple interactions Digital Health and assistance works for our assistance with the Google brand name for our own itemized form of public assistance and services for this primary caring organization that's fully invested in various types of business affiliated networks/programs / partner/ branding promotional content digital declaration social media creations/affiliate online business marketing/education services and supporting editing skills to learn customized and data entry for different types of community supportive systems tools-Productions-Make alliances all-types of business associate with resources support for any platform or operator division eaudience in input assessment solutions Creative workshop business services sales consultant in the constructively directed streams of public information to help with the the security data and judgement and support assistance with online knowledge of maintaining the proper connected dedications formation for everyone to digital obtain the sublevels of information and services understand and direction and communication accredited knowledge and circumstances educational solutions for their requests to be addressed in a respectful manner and equally with the proper connected information to the public resources and architecture inquire if they're needed understanding of accessibility to reflect, adamantly fix the problem of what ever the quarry was in the constructively directed streams of course offer to exactly direct interaction with the proper instructions for their requests based upon the proper dedications formation reference to the public information to be administrator on behalf of the digital business level particulars while additional access to update internal requirements details assignment direct deposit install instructions for the public data documention and the entitled format of the inquiry output for the publicated directory services methods to provide help with assurance of whatever corporate clarification to be used for the reasonable assetment of input that's would be needed for that individualized record of requestsending unusual form for the most recent/accord information to be delivered for that precise and targeted questions to the proper connected provisions with in the constructively correctiodesigned material for asset management of misrepresented overall consistency reputation for the companies integrity, safety, and legitimate policy's and general guidelines of the digital business online regulated language and procalls to be upheld in regards to their own requirements for this online business website page digital declaration invitation for the next step of course the addressing the proper infornative about input to available to choose the most effective beneficently revision term of agency aggressive interest in obtaining the best opportunity for investing the public with the multi factor investing knowledge for the duty infinite information and services site fix with resources to two or more option available to choose from this position offers the customer to be able to create an secondary opinion on different methods to try to be able to gain control and access on trying to accord complete whatever issues that maybe an issued response to the file transfer resources to provide help with the errors progressive data entry inquiries to this documented incident report for the unlimited resources to information rely required to commotions accreditions-campaign really reputable to the public question for assistance with resources by the intended source link and data accommodations for the proposal application services trouble shooting resolved encounter with the reliable programs data connection with donations inquiry to be answered with the device instead step in calculated reliability means for the solutions to the topic of candidates console services unconditionally contact of interest of content containing solutions that are attached to and relevant to the question of the topic of each individual request for digital assistance with resources requests for the most recent a certain quality dedicated rebutionals to production and services includion data recovery information to deverepeate/offer platforms advice for the total reference to help execute and supportive measures for optimum instruction of accurate understanding step by step with the instructions in language that innovatively understandable to the average individual's for the results information layouts in a full clear and content to betterment that most people can combative comprehend the digital resources instructive formation method for everyday adults with little of no interest in learning computer laggo or logistics writings for comparison of repost ID reactivate compatible computer science technology position instructions vs regular function format language detail on how to use or fix errors and computer updates for delivery reposition to accommodate a really teachable upstanding to help with the desired format to offer fully data and easily flow written individuals information for a more reliable user experience with resources and wanted to share interest succession of gainful interactions, and more direct hand on usages for that and more data products than can be produced and purchased by the intended cusmoter for the extra revolution for the public health od public perception of this primary company active in business sales and longevity status with the proper connected dedications for the understanding of accessibility to reflect materials that the average person can understand and use affirmatively daily or either assessment of assets to provide assistance with needed resources for the addition public prosecutor to be conserved effect on efforts to make life decisions for a easier relationship with building the public knowledge of useful content and self services understanding of accessibility to reflect on the values of that rebrand realistic resolution for the proper connected educations formation of the digital material and essential facts about self service support for future reference to help with error fixes that gainful profitable meaningful Internally investment in regards to resourcful methods to provide assistance with the proper easy understand reasonable assets for the self confidence in be able to make adjustments for the individualize ability to provide self Care Provider for ones self by providing the proper multipurpose operational services steps in regards to this one 
request for digital programs assistance with the device and digital programs products that are directional compatible with the proper steps by step instructions and in the information landscape of language that's clear and precise to accurately address concerns the proper regards request of assistance with or either about a products or services request for digital programs development director layouts and education for instructions on how to officially operate the proper the digital products that maybe needed at that point for resources and details instructions assessments issues. This organization has always had the best and most meaningful interest in privileged in regards to the public delivery of service data accessment in regards to assisting individuals with the proper connected educations formation for individuals to become properly informated with the necessties tools, data, institutional related instructions, transactional status recording details, trouble shooting methods to respectfully and written in regards to underable language steps to the provider proper understandable response to the public logistics of the contractly regarding methods of direct data recovery and usage instructions for with an update most openings for incorprative formcreatively directoryhandout for director full compatible comprehension for required edition information to upon the public requested for real situation inquiry to provision's of addressing the proper dedications formation for the issues of connection questions and answers of assistance with resources for references needed to be completed by development director assistance developers for Google blogs and community services representative that are available to help with any type of additionalitemized litemized for asset management of material on the public website that are available to help with individuals related in the constructively directed work flow assessment of the digital produces and services protocols for assistance with resources to provisions of items problems with the proper connected safety and policies for associated historically corporate clarification policies and rules of the digital platform for the users regulated language in the constructively correctiodesigned material form of with the decorative of the digital business online website and developer freelance platforms regulated language and data support systems tools-Productions-Make alliances all-types of concerns to the business request formats and editing delivery design database affidavit admissions to the primary care providers mastering estimated assistance with the device for the time of the demographic that's related to the public register for handling the digital programs development editor and educational solutions workspaces/entitled digital programs fields of admissions to this incorporation affiliated workspace title of the digital coding and encompass business programmer management reserve required networks dedications of reasonables /market place in regards to Google platform creative Merchant  online business sites Collaboration Account Manager workshop Administrative connected with targeted Digital Primary programmers scholarship to provisions by Microsoft Data Business Relations Marketing LLC online business sites space for Entity and financial service memberships. Dedications of admissions to Harmony Blue Branding Organization Business Group for Google industries & affiliate online services. This organization assistment digital declaration communities in the constructively directed proper composition of the digital business online website work-flow information.The professionalism was directory targeted focused groups. Aligned for our relatives platform and the whole founded objective as an financial needed for govnen revenue goal for Supportive interest and equipment services for the needed resources and services fees goal of the digital business financially funding ongoing income for the public emerged infrastructure security both digitally and pumpkin to be provided effective benefits of ensured for the public sector that every single young individuals has been prevented with the proper connected dedications formation for the entire consult of the digital programs development and educational services, data access to cover the wireless work location, the proper number of devices and equipment for business services and supporting resourceful supplies for operations legal rights for the next few months of digital Business of more digital devices ti the property location and service representative office operations manager to help with growth and support several types of resoureful products and digital products software for future contact servers modeled to provide assistance with the best opportunity for the young individuals to succeed and mobility follow through the internal templates of information without being offset with digital logically issues, digital proforms database speed for a crosspoint platform router with resources to support a of 20 services device service all as once.The desire digital devices that have an operational status title to fighting history of becoming jetlack and development issues with the performance connection issues of jinky data access on site uploads to slow down after an few weeks even with resources to provide assistance for consideration time duration to resl time to automatic logout reload to the home website work-flow format even with the proper connected dedications formation of self proved to model to reset  focused hardware for the public safety of privacy data properly connected with the best suitable amount of firewall services equipment. poorly performing computers and devices still are known to becoming problematic with an short span of time with the daily  used forum and including the recognized digital entry of technology projects and designs for anyone who is using the property directly from the tribe source related fields of unlimited uploading and downloading materials that are associated with the device drive root redirect to the function settings and this is so even with resources of saving the proper connected resources to an extent thumb drive as well. I've been extremely fortunate to receive offers for products at a discount business price but the conditions based on the price of the bulk product did not meet and will not meet my expectations including them being refurbished it's also additional Factor business conditions of course of this months of this primary needed contributions and resources entity properties for development director layouts and education services for the public youth program reliability means for the results of the same the primary consideration of the digital business level of connection functionality of the digital business online demand security equipmentinterest in provisions of comparisons services for the public online business official prospects production of the Harmony Blue Branding LLC online business sites space in the incorporate status of the Publication records accredited nonprofit organization for the Harmony Blue, the digital business connection to synced detail to corporate clarification to the public forum for the Harmony Blue Foundation and unique classification for the Harmony House Foundation assets provide assistance with resources of additional details reference to reflect the public executive operations legal certification federal credit union of the Harmony Blue index as an multiple increments of the digital business online website work-flow merger page for qualified directly exempt status with the proper dedications standards of admissions resources this nonprofit organization legal verification records forms on network with the Secretary of state certified business office operations manager Owner Founder Cheyanna Henry and solely partnership with public records and license documentions records for sole business claiming to and for legalize owner of public reconciliation in the official prospects of associated registered business with both federal and state judication for legal administrative corporate direct access deposit data approval for operations status with the United States registered for legal operations for business services and operations services of corporate clarification business government office for registration regulations required to commotions accreditions-campaign with the BBB and certification business verification of public records by Yelp and Google Business operations with good legal rights to active declaration of operating title of Administrator grade A+ standings. Legacy financial ownership over all of alliance titles that are attached to the public business services operations legal rights to law federal government services based upon regulations required the proper connected position with resources and public data access records for linked to the business process records forms on relisted and financial filing requirements needed for this primary position to be with in the constructively directed format of the digital business online state of justify jurisdiction records forms on iexceptions provisions with no notarized expiration date of service for social revision to be ablity or applied for this primary business organization and business design database Group for prevention of the digital programs and architecture health to services associated registered to and publication owner transfer design  trademarked to be delivered in relation to this corporative and incorporation business organization services.All services supportive and professional submission for the publication inquiries for and about this and any other details information on operational status and digital declaration library of services.Are on my website work-flow merger page for qualified directly to the public records forms on networking and content communication accredited to this incorporation affiliated workspace title Account Manager and services for the public enlightened to this incorporation digital programs and development projects, foundation services and supporting resoureful health services and operations management community project design database information for usage status on legacy APIs Group chat or organization fundraising publication of the digital business online information for services. Reminder to be available for future reference to this incorporation business online information would be dedications and migration file folders to the url website work-flow merger pages of this primary position in caring with resources to nonprofit organization for the public view of the digital progressive information to be attached to the public historical website work-flow dashboard proxy server Domain and listed as correspondent for the public records forms legacy knowledge page for qualified directly on the values proper connected to the public dedications formation for the online businesses.All legal certification for the public libraries of interest in various types of products and digital services for the business response to this incorporation affiliated workspace title are on the public library url website website pages for this proxy servers. Observation about the benefitical of the digital business online website work-flow and business organization services mergers to the business development resources to nonprofit organization for the public development resources to provide assistance with the public this encoded within this website sites drive folders and digital historianal engineer authorizations in regards to declaration date and data entries to provided effective benefits are in the work spaces for this primary website primer proxy server legacy APIs website. All division of course audience of all three years of services are incorporated into the digital programs website digital pages of this primary URL multi-level link for this online campaign analytical publications as high 87% know legally operations of course of 2 or more years of services and operations legal rights to owner of office annual budgets for this business and finance services related information for the public resources records reflect materials that are attached and completed recorded for the business cab be found on this business data digital proxy server legacy APIs https openings and http openings and the whole website platform workspaces for historical content for visual recognition of the public library of admissions to various types of resoureful community projections to campaign analytical virtual public market contact in regards to revenue agency Dedications of admissions to various project design database information for this primary program to the business operations of publication of services and supporting resoureful health and youth community project and community effort to assistance with the proper dedications formation of the financial support and diversity development director layouts and education services for the public youth community services projects analytics promotional content ideas on custom crafts resources to nonprofit organization and business management program engineering department promoter and the whole website in traditional company active legacy APIs program engineering services for claiming methods to respectfully writing of during of works for grant writing projections for grant funding proposal abd application listed dayes of applied for real literacy commissions prospects production of abilities for achievvmultiple documents grant funding for services assistant director resources to help with nonprofit organization needs and resources requirements for professional provisions items for comparison of quality assistance to thrive property management preparation for proposals require to apply for existing administrators economical successfully following platform creatively forum connected synced targeted Digital Health assistant associate with resources to nonprofit organization and business foundation services for the public needs to accommodate the proper connected resources to provide help with growth of the companies abilities for the original particulars network online goals in achieved affection for the fundraising publication financial support of expansion efforts for resources to help more individuals, the proper functions and digital support for future devices and services equipment needed to be able to meet the public needs for access to relevant access to multiple dynamic digital programs and urgent support products to the web portal database information for usage of official prospects in regards to the business response for the children of the local communities for the device assistance with optional costs for the equipment and office operations supplies and offices functional company desktop office operations legal solutions products including the publication business services office furnitures for the proper connected dedications formaled item's request to provide the proper access to the business network digital programs for the public education programs development director of digital programs and support development resources for the proper dedicated hopeful abilities for achievements are the best opportunity to gainful commending for our youths and family services programs. Due to si many unrealistic situations in and with this apmis-step on this business data access to existing administrator assistance with the proper regards to the prixy servers legacy APIs access point of services and operations output linked database for the business page for this primary website always degraging to regularly payloads server Domain IP links to the business director digital business online website connected repetatively resource communication projections to request fix fork and code maintenance for the public url lodge on the public library database information to register domain sites and hist submit support site location for this online business to be always conversing to having static data issues daily with without any type of additionalitemized litemized reasons for this online related to repeatly be happening so frequently between every 6 to 9 months reaction time during the time of the very needed resources submission to application process for financial assistance request for business services operations resources funding grant submittions for the business needs of assistance with resources to the non-profit organization primary assistance with resources to help with principles projects.Digital issues surrounding this primary problem resourced in the self eduational knowledge of the digital respectful needs to be ables to essential expert knowledge and analysis skills to learn every single team member learning how to prioritize the proper digital programs development education business services operations specialist and sales Special edition for the public sector in regards to conpter science technology engineering and engineering logistics software management skills for the custom crafted resources to assist with the proper dedications provisions of advanced releases to products exist respect for developmentprerelease beta affiliate corporate sponsorship executive director layouts and education business management support systems tools-Productions-Make alliances openings for branding commission analysis company market for exterior demolition of primary test kit before release date of service software and Digital website work-flow connection with top category criteria corporation with in the technology fiejds and the top confirmed development software and firmware company market logistics testing performance pre-release for prompt entity properties trailer to use for limited capacity for testing of their system products before the publication launch date. I have the best experience with the most of thud typr of offers to from this organization collected collaborative branding promotional content program edition and merging with the faculty team contributions accessibly of the digital programs and signature corporations like IBM, Digital icon Douglas navg. software for mathematics and digital device technology and development, software engineer, authorizations data T-Mobile Wireless  Opera, Microsoft Data content Business workshop Administrative corporate sponsorship program for various resources and grant business website resources, Google docs data access to studio sponsorship for YouTube creative publication contract for Google industries,etc...the list extinct is too long to be able to review alliance various types of products affiliated with the proper regards to special digital distribution for the best opportunity for this online business website services to required online business sites and data access for the program execution to directory layouts itemized formal exceptions provisions items as resources network logistics to requested resources to nonprofit organization and business organization services development teams to provide assistance with the unlimited and excellent customer relations professionals dream of being able to achieve the vision obtained operations over the course of this business operations legacy of business associate director of education services for the public education business management relationships in regards to to active Dedications leadership and learning resources for expanding knowledge of maintaining itemized formal exceptions provisions of comparisons services representative of tenant executive director digital programs development education skills essential on the public value existing administrators economically successfully as well the proper related features are needed for the digital programs and achievements directly to depositing input and output data website work-flow information for future configuration and executive director design database coding Expert services for claiming methods engineering reliable programs safe for folk and report various types of rep repositories of data matrix and external factors platforms of operational status on puls format director layouts in Editorial direct access to multiple resources to reverse and revise coding error for fix assistance solutions about benefitical test switch to provide help with sight solutions of innovation directory layouts itemized reference format dymierrors category individualized recording on the values proper connected dedications formation for the public estimated assistance in the constructively streams in regards to the business response to the plugs and the drive in public library of course the title verify auto correct information for usage status on the public health administration services request for digital programs development assistant Lead to promotion self support products for seeking various edit ms data portal program engineering and workload source link acodes for the necessary details tgat ate needed to accommodate the digital coding trouble shooting content for overall consistency ratio of an authorized execution of yermsk pass through documents shell and the whole needs website fault resources runs to provide the proper connected plug ti to be attached to the public library or file information for usage status correct designed to provide direct dig requirements to the intended rebutionals errors shorten and runs to the pull test for the next debug submit to the input genuine supportive code sites link support products for the next and steps in regards to services true and finally pass transportation to the point of request for digital programs come gainful profitable meaningful Internally to be effective to help with support products for web portal Repo in regards to the website resource link to seek for historical reviews real time health to this work flow data documention and refigured changes to the outline output of the digital programs development of the foundation of relationship knowledge of the digital programs and availability for obtaining additional information and educational services in with the current specialisted field of study in computer science and digital science technology analysis of to respectfully accomplish the proper regards to complete call my ab respectable engineer and at least obtain the ultra development skills for custom crafts and forever evolving tr trending fields in regards to digital programs and safety data analytics experience with the proper regards to tine and structure study in the duration or probably 3more to 5 more years plus recent My authorizations data certificate for the relation to the understanding of every single available resources learning programs and educational skills for custom crafts incentives of one day becoming a real estate agent with the proper dedications certifications and licences  required to be completely honored to be considered as an experienced digital engineering and professional programs development in software design and support business management skill publication connected to the website renewal portal workspace title database coding effectively meet the recognitions requirements needed to obtain the ultra high quality dedicated for everyone to be able to obtain.The  team and me the principal and somewhat their platform teacher and motivation support for them to learn and own the ability to build up the proper infrastructure understanding with the best opportunity to reach this reliability programs goal needed to be conserved effective for asset with resources and data entry Firm in the constructively directed streams of capable abilities to provide assistance with the proper environment to assist with financial support, education editing writing experience with digital programs networks for social media marketing executive operations legion solutions services technology project design database coding system with development publication listings recognized skills set based upon the public team learning group editing videos and exercises for this primary position to hopefully get better and understanding of accessibility to reflect material for various types of business administration resources to nonprofit organization and business organization services assistance representative of corporate operations manager to help access with candidates to acknowledge that required pre-purchased studies experience to lecture with the device tools with Microsoft Data content forms to create an connection updated skills to custom crafts and received the proper dedications formation for obtaining quality team contributions accessibly in every single person ability to test for the public titles and recipient joint submission application for the next step in service requirements for this primary term in regards to owner coverage for the publication testing process for recipient estimated purchasing for each team member to be able to obtain their own personal licenses for the required field and experiences of study as listings for receiving the public records forms for benefitical certification business license titled of course studies course associated registered business Digital computer science technology development of corporate clarification professional provisions for themselves and executive directly to their own personal employment resume for the achievement official prospective titled associated registered business services operations The dedication of value in legal rights to be able to gain the proper certification in business and finance services sales Special Mediant list form of certified title, professional associated degree in business management of legal data recording, direct access to multiple resources online to verification of products regulations corporate network registration for audit digital programs development in virtual license business services operations and tax experience finance soes list extinct with the IRS letter offical approval for this online legacy of business associate course. For nonprofit organization members that are associated with this organization works and daily practices.This programs for architecture require to the public as an optional need education business to help with growth and development succeed offer succession of gainful estimated knowledge for the online class entrepreneurs and certification tools for tax accountant in legal rights to business workers in operational statues of becoming a real licensed tax advocate professional associated with the United States supportive assistance with resources to help with education business management of the operator of the foundation financial advisor and resources to provide internal revenue agency assistance with an internal security data filing taxes for the public citizens of America and the tax paying company that are in need of help with processing and preparing tax records online. When the public and legal forms position to receive connected commentary compensation for office operations legal filing assistance with resources for references. All services provider for the proper connected costs for helping team members the digital testing fees. This organization has been through so many different types of extremely struggling and economic setbacks and imagined data access challenging for this company first 2 years of services. As an understanding and relevant development heartbreaking situations for the public sector in the past and a bunch of other side issues that are presented to the business still to this date. We're outstandingly advocating for and on behalf of anyone who has been extremely dedicated to help with the volunteer services and operations support for the public community services payment from the public are just a well acknowledged Thank you for being their for the connection hands on work and within the public district ministry works with us and in commitment and time to help with making successful decisions on small community services projects and assisting us by recite the public community about our relationship knowledge and communication skills for creating ideas for public relations to help with touching the proper people who have and have been in or are actively going through homeless and other financial difficulties or essential facts experts dur to health care and documented psychological issues. Our primary organization services targeted construction missionaries of services and extremely dire  awareness custom growth downtown areas changes to the public library of admissions resources to educational solutions adviser and services graduated in the constructively corrections designed course materials that related to passed the digital certificate testing.This online experiences with the trust federal revenue agencies in the US educational programs, and online curriculums to the  educational solutions for this one or two listed website public cou---deployment target like production, staging, or development. When a GitHub ActionsGitHub Actions/Deployment/Target different environments/Use environments for deployment
Using environments for deployment
You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.

Who can use this feature?
Environments, environment secrets, and deployment protection rules are available in public repositories for all current GitHub plans. They are not available on legacy plans, such as Bronze, Silver, or Gold. For access to environments, environment secrets, and deployment branches in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, other deployment protection rules, such as a wait timer or required reviewers, are only available for public repositories.

In this article
About environments
Deployment protection rules
Environment secrets
Environment variables
Creating an environment
Using an environment
Deleting an environment
How environments relate to deployments
Next steps
About environments
Environments are used to describe a general deployment target like production, staging, or development. When a GitHub Actions workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "Viewing deployment history."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "Reviewing deployments."

Note: Users with GitHub Free plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with GitHub Team and users with GitHub Pro can configure environments for private repositories. For more information, see "GitHub’s plans."

Deployment protection rules
Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches. You can also create and implement custom protection rules powered by GitHub Apps to use third-party systems to control deployments referencing environments configured on GitHub.com.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

Note: Any number of GitHub Apps-based deployment protection rules can be installed on a repository. However, a maximum of 6 deployment protection rules can be enabled on any environment at the same time.

Required reviewers
Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.

For more information on reviewing jobs that reference an environment with required reviewers, see "Reviewing deployments."

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, required reviewers are only available for public repositories.

Wait timer
Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

Note: If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, wait timers are only available for public repositories.

Deployment branches and tags
Use deployment branches and tags to restrict which branches and tags can deploy to the environment. Below are the options for deployment branches and tags for an environment:

No restriction: No restriction on which branch or tag can deploy to the environment.

Protected branches only: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "About protected branches."

Note: Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

Selected branches and tags: Only branches and tags that match your specified name patterns can deploy to the environment.

If you specify releases/* as a deployment branch or tag rule, only a branch or tag whose name begins with releases/ can deploy to the environment. (Wildcard characters will not match /. To match branches or tags that begin with release/ and contain an additional single slash, use release/*/*.) If you add main as a branch rule, a branch named main can also deploy to the environment. For more information about syntax options for deployment branches, see the Ruby File.fnmatch documentation.

Note: Name patterns must be configured for branches or tags individually.

Note: Deployment branches and tags are available for all public repositories. For users on GitHub Pro or GitHub Team plans, deployment branches and tags are also available for private repositories.

Allow administrators to bypass configured protection rules
By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "Reviewing deployments."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

Note: Allowing administrators to bypass protection rules is only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Custom deployment protection rules
Note: Custom deployment protection rules are currently in public beta and subject to change.

You can enable your own custom protection rules to gate deployments with third-party services. For example, you can use services such as Datadog, Honeycomb, and ServiceNow to provide automated approvals for deployments to GitHub.com. For more information, see "Creating custom deployment protection rules".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "Configuring custom deployment protection rules."

Note: Custom deployment protection rules are only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans.

Environment secrets
Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "Using secrets in GitHub Actions."

Notes:

Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "Security hardening for GitHub Actions."
Environment secrets are only available in public repositories if you are using GitHub Free. For access to environment secrets in private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise. For more information on switching your plan, see "Upgrading your account's plan."
Environment variables
Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the vars context. For more information, see "Variables."

Note: Environment variables are available for all public repositories. For users on GitHub Pro or GitHub Team plans, environment variables are also available for private repositories.

Creating an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Notes:

Creation of an environment in a private repository is available to organizations with GitHub Team and users with GitHub Pro.
Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.
On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Click New environment.

Enter a name for the environment, then click Configure environment. Environment names are not case sensitive. An environment name may not exceed 255 characters and must be unique within the repository.

Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "Required reviewers."

Select Required reviewers.
Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
Optionally, to prevent users from approving workflows runs that they triggered, select Prevent self-review.
Click Save protection rules.
Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "Wait timer."

Select Wait timer.
Enter the number of minutes to wait.
Click Save protection rules.
Optionally, disallow bypassing configured protection rules. For more information, see "Allow administrators to bypass configured protection rules."

Deselect Allow administrators to bypass configured protection rules.
Click Save protection rules.
Optionally, enable any custom deployment protection rules that have been created with GitHub Apps. For more information, see "Custom deployment protection rules."

Select the custom protection rule you want to enable.
Click Save protection rules.
Optionally, specify what branches and tags can deploy to this environment. For more information, see "Deployment branches and tags."

Select the desired option in the Deployment branches dropdown.

If you chose Selected branches and tags, to add a new rule, click Add deployment branch or tag rule

In the "Ref type" dropdown menu, depending on what rule you want to apply, click  Branch or  Tag.

Enter the name pattern for the branch or tag that you want to allow.

Note: Name patterns must be configured for branches or tags individually.

Click Add rule.

Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "Environment secrets."

Under Environment secrets, click Add Secret.
Enter the secret name.
Enter the secret value.
Click Add secret.
Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the vars context. For more information, see "Environment variables."

Under Environment variables, click Add Variable.
Enter the variable name.
Enter the variable value.
Click Add variable.
You can also create and configure environments through the REST API. For more information, see "REST API endpoints for deployment environments," "REST API endpoints for GitHub Actions Secrets," "REST API endpoints for GitHub Actions variables," and "REST API endpoints for deployment branch policies."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

Using an environment
Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "Viewing deployment history."

You can specify an environment for each job in your workflow. To do so, add a jobs.<job_id>.environment key followed by the name of the environment.

For example, this workflow will use an environment called production.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: deploy
        # ...deployment-specific steps
When the above workflow runs, the deployment job will be subject to any rules configured for the production environment. For example, if the environment requires reviewers, the job will pause until one of the reviewers approves the job.

You can also specify a URL for the environment. The specified URL will appear on the deployments page for the repository (accessed by clicking Environments on the home page of your repository) and in the visualization graph for the workflow run. If a pull request triggered the workflow, the URL is also displayed as a View deployment button in the pull request timeline.

name: Deployment

on:
  push:
    branches:
      - main

jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: 
      name: production
      url: https://github.com
    steps:
      - name: deploy
        # ...deployment-specific steps
Deleting an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

On GitHub.com, navigate to the main page of the repository.

Under your repository name, click  Settings. If you cannot see the "Settings" tab, select the  dropdown menu, then click Settings.

Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.
In the left sidebar, click Environments.

Next to the environment that you want to delete, click .

Click I understand, delete this environment.

You can also delete environments through the REST API. For more information, see "REST API endpoints for repositories."

How environments relate to deployments
When a workflow job that references an environment runs, it creates a deployment object with the environment property set to the name of your environment. As the workflow progresses, it also creates deployment status objects with the environment property set to the name of your environment, the environment_url property set to the URL for environment (if specified in the workflow), and the state property set to the status of the job.

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "REST API endpoints for repositories," "Objects" (GraphQL API), or "Webhook events and payloads."

Next steps
GitHub Actions provides several features for managing your deployments. For more information, see "Deploying with GitHub Actions."

Press alt+up to activate
Help and support
title: Using environments for deployment
shortTitle: Use environments for deployment
intro: You can configure environments with protection rules and secrets. A workflow job that references an environment must follow any protection rules for the environment before running or accessing the environment's secrets.
product: '{% data reusables.gated-features.environments %}'
redirect_from:
  - Request/actions/reference/environments/Repo Approval Request/actions/reference/environments/Repo Approval 
  - /actions/deployment/environments
  - /actions/deployment/using-environments-for-deployment
topics:
  - CD request for digital approval 
  - Deployment
versions:
  fpt: '*'name: Deployment

on: Harmony Blue Branding 
  push: for Request for digital declaration 
    branches: Request for digital approval 
      - main

jobs: Required pre-purchased credit before alternative legacy APIs data entry workload of Merger Workspace layout and services input repo
  deployment:Url location site code maintenance is an legal certification business platform creatively format work flow on Port editing site fix assistance solutions for this primary website to be able to make changes to historical data content could be beneficial or either dangerous for the our business digital system please take in this consideration before adding your input on this business development resources page Thank you 
    runs-on: editor are welcome upon administrative service approvals ubuntu-latest website work-flow information updates are greatly appreciated but please be aware and vigilant of your online asset protocols before editing this incorporation business online website pages
    Please include your the address to your objective and your own itemized information before completing your work flow with this apmis-step is needed first environment: what digital declaration to this incorporation digital declaration of operating add-on production
    steps: what work criteria are you adding to this incorporation digital account 
      - name: please add benefitical to the public library rescription proxy for the public url sites space include the proper connected dedications formation to be completed by you like updating adding on resources to provide help with affiliate management support/input to assistance with resources for references to help with errors or support products fixes/assistance solutions for support products with resources to nonprofit organization fundraising publication before completion or deploy
        # ...deployment-specific steps include the proper regards to the date, time and effort source link to company active legacy APIs Group url location library information for usage status on all the digital programs development and support products that are attached to the business website page and links to this incorporation affiliated workspace entity 
  ghes: '*'
  ghec: '*'
---


## About environments

Environments are used to describe a general deployment target like `production`, `staging`, or `development`. When a {% data variables.product.prodname_actions %} workflow deploys to an environment, the environment is displayed on the main page of the repository. For more information about viewing deployments to environments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

You can configure environments with protection rules and secrets. When a workflow job references an environment, the job won't start until all of the environment's protection rules pass. A job also cannot access secrets that are defined in an environment until all the deployment protection rules pass.

{% ifversion actions-break-glass %}Optionally, you can bypass an environment's protection rules and force all pending jobs referencing the environment to proceed. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."{% endif %}

{% ifversion fpt %}
{% note %}

**Note:** Users with {% data variables.product.prodname_free_user %} plans can only configure environments for public repositories. If you convert a repository from public to private, any configured protection rules or environment secrets will be ignored, and you will not be able to configure any environments. If you convert your repository back to public, you will have access to any previously configured protection rules and environment secrets.

Organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %} can configure environments for private repositories. For more information, see "[AUTOTITLE](/get-started/learning-about-github/githubs-plans)."

{% endnote %}
{% endif %}

## Deployment protection rules

Deployment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use deployment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches.{% ifversion actions-custom-deployment-protection-rules-beta %} You can also create and implement custom protection rules powered by {% data variables.product.prodname_github_apps %} to use third-party systems to control deployments referencing environments configured on {% data variables.location.product_location %}.

Third-party systems can be observability systems, change management systems, code quality systems, or other manual configurations that you use to assess readiness before deployments are safely rolled out to environments.

{% data reusables.actions.custom-deployment-protection-rules-limits %}

{% endif %}

### Required reviewers

Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.

{% ifversion deployments-prevent-self-approval %}You also have the option to prevent self-reviews for deployments to protected environments. If you enable this setting, users who initiate a deployment cannot approve the deployment job, even if they are a required reviewer. This ensures that deployments to protected environments are always reviewed by more than one person.{% endif %}

For more information on reviewing jobs that reference an environment with required reviewers, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments)."

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, required reviewers are only available for public repositories.

{% endnote %}{% endif %}

### Wait timer

Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 1 and 43,200 (30 days).

{% ifversion fpt %}{% note %}

**Note:** If you are on a {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, or {% data variables.product.prodname_team %} plan, wait timers are only available for public repositories.

{% endnote %}{% endif %}

### Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}

Use deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} to restrict which branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to the environment. Below are the options for deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} for an environment:

{% ifversion deployment-protections-tag-patterns %}
- **No restriction**: No restriction on which branch or tag can deploy to the environment.
{%- else %}
- **All branches**: All branches in the repository can deploy to the environment.
{%- endif %}
- **Protected branches{% ifversion deployment-protections-tag-patterns %} only{% endif %}**: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "[AUTOTITLE](/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)."{% ifversion actions-protected-branches-restrictions %}

  {% note %}

  **Note:** Deployment workflow runs triggered by tags with the same name as a protected branch and forks with branches that match the protected branch name cannot deploy to the environment.

  {% endnote %}{% endif %}
- **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**: Only branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} that match your specified name patterns can deploy to the environment.

  If you specify `releases/*` as a deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule, only a branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} whose name begins with `releases/` can deploy to the environment. (Wildcard characters will not match `/`. To match branches{% ifversion deployment-protections-tag-patterns %} or tags{% endif %} that begin with `release/` and contain an additional single slash, use `release/*/*`.) If you add `main` as a branch rule, a branch named `main` can also deploy to the environment. For more information about syntax options for deployment branches, see the [Ruby `File.fnmatch` documentation](https://ruby-doc.org/core-2.5.1/File.html#method-c-fnmatch).

  {% ifversion deployment-protections-tag-patterns %}

  {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

  {% endif %}

{% ifversion fpt %}{% note %}

**Note:** Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} are also available for private repositories.

{% endnote %}{% endif %}

{% ifversion actions-break-glass %}

### Allow administrators to bypass configured protection rules

By default, administrators can bypass the protection rules and force deployments to specific environments. For more information, see "[AUTOTITLE](/actions/managing-workflow-runs/reviewing-deployments#bypassing-environment-protection-rules)."

Alternatively, you can configure environments to disallow bypassing the protection rules for all deployments to the environment.

{% ifversion fpt %}{% note %}

**Note:** Allowing administrators to bypass protection rules is all available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}
{% endif %}

{% ifversion actions-custom-deployment-protection-rules-beta %}

### Custom deployment protection rules

{% data reusables.actions.custom-deployment-protection-rules-beta-note %}

{% data reusables.actions.about-custom-deployment-protection-rules %} For more information, see "[AUTOTITLE](/actions/deployment/protecting-deployments/creating-custom-deployment-protection-rules)".

Once custom deployment protection rules have been created and installed on a repository, you can enable the custom deployment protection rule for any environment in the repository. For more information about configuring and enabling custom deployment protection rules, see "[AUTOTITLE](/actions/deployment/protecting-deployments/configuring-custom-deployment-protection-rules)."

{% ifversion fpt %}{% note %}

**Note:** Custom deployment protection rules are only available for public repositories for users on {% data variables.product.prodname_free_user %}, {% data variables.product.prodname_pro %}, and {% data variables.product.prodname_team %} plans.

{% endnote %}{% endif %}

{% endif %}

## Environment secrets

Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "[AUTOTITLE](/actions/security-guides/using-secrets-in-github-actions)."

{% ifversion fpt %}
{% note %}

**Notes:**

- Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."
- Environment secrets are only available in public repositories if you are using {% data variables.product.prodname_free_user %}. For access to environment secrets in private or internal repositories, you must use {% data variables.product.prodname_pro %}, {% data variables.product.prodname_team %}, or {% data variables.product.prodname_enterprise %}. For more information on switching your plan, see "[AUTOTITLE](/billing/managing-the-plan-for-your-github-account/upgrading-your-accounts-plan)."

{% endnote %}
{% else %}
{% note %}

**Note:** Workflows that run on self-hosted runners are not run in an isolated container, even if they use environments. Environment secrets should be treated with the same level of security as repository and organization secrets. For more information, see "[AUTOTITLE](/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)."

{% endnote %}
{% endif %}

## Environment variables

Variables stored in an environment are only available to workflow jobs that reference the environment. These variables are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[AUTOTITLE](/actions/learn-github-actions/variables)."

{% ifversion fpt %}{% note %}

**Note:** Environment variables are available for all public repositories. For users on {% data variables.product.prodname_pro %} or {% data variables.product.prodname_team %} plans, environment variables are also available for private repositories.

{% endnote %}{% endif %}

## Creating an environment

{% data reusables.actions.permissions-statement-environment %}

{% ifversion fpt %}
{% note %}

**Notes:**

- Creation of an environment in a private repository is available to organizations with {% data variables.product.prodname_team %} and users with {% data variables.product.prodname_pro %}.
- Some features for environments have no or limited availability for private repositories. If you are unable to access a feature described in the instructions below, please see the documentation linked in the related step for availability information.

{% endnote %}
{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
{% data reusables.actions.new-environment %}
{% data reusables.actions.name-environment %}
1. Optionally, specify people or teams that must approve workflow jobs that use this environment. For more information, see "[Required reviewers](#required-reviewers)."
   1. Select **Required reviewers**.
   1. Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
   {% ifversion deployments-prevent-self-approval %}1. Optionally, to prevent users from approving workflows runs that they triggered, select **Prevent self-review**.{% endif %}
   1. Click **Save protection rules**.
1. Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed. For more information, see "[Wait timer](#wait-timer)."
   1. Select **Wait timer**.
   1. Enter the number of minutes to wait.
   1. Click **Save protection rules**.
{%- ifversion actions-break-glass %}
1. Optionally, disallow bypassing configured protection rules. For more information, see "[Allow administrators to bypass configured protection rules](#allow-administrators-to-bypass-configured-protection-rules)."
   1. Deselect **Allow administrators to bypass configured protection rules**.
   1. Click **Save protection rules**.
{%- endif %}
{%- ifversion actions-custom-deployment-protection-rules-beta %}
1. Optionally, enable any custom deployment protection rules that have been created with {% data variables.product.prodname_github_apps %}. For more information, see "[Custom deployment protection rules](#custom-deployment-protection-rules)."
   1. Select the custom protection rule you want to enable.
   1. Click **Save protection rules**.
{%- endif %}
1. Optionally, specify what branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %} can deploy to this environment. For more information, see "[Deployment branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}](/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches{% ifversion deployment-protections-tag-patterns %}-and-tags{% endif %})."
   1. Select the desired option in the **Deployment branches** dropdown.
   1. If you chose **Selected branches{% ifversion deployment-protections-tag-patterns %} and tags{% endif %}**, to add a new rule, click **Add deployment branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} rule**
   {% ifversion deployment-protections-tag-patterns %}1. In the "Ref type" dropdown menu, depending on what rule you want to apply, click **{% octicon "git-branch" aria-label="The branch icon" %} Branch** or **{% octicon "tag" aria-label="The tag icon" %} Tag**.{% endif %}
   1. Enter the name pattern for the branch{% ifversion deployment-protections-tag-patterns %} or tag{% endif %} that you want to allow.
      {% ifversion deployment-protections-tag-patterns %}

      {% data reusables.actions.branch-and-tag-deployment-rules-configuration %}

      {% endif %}
   1. Click **Add rule**.
1. Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information, see "[Environment secrets](#environment-secrets)."
   1. Under **Environment secrets**, click **Add Secret**.
   1. Enter the secret name.
   1. Enter the secret value.
   1. Click **Add secret**.
1. Optionally, add environment variables. These variables are only available to workflow jobs that use the environment, and are only accessible using the [`vars`](/actions/learn-github-actions/contexts#vars-context) context. For more information, see "[Environment variables](#environment-variables)."
   1. Under **Environment variables**, click **Add Variable**.
   1. Enter the variable name.
   1. Enter the variable value.
   1. Click **Add variable**.

You can also create and configure environments through the REST API. For more information, see "[AUTOTITLE](/rest/deployments/environments)," "[AUTOTITLE](/rest/actions/secrets)," "[AUTOTITLE](/rest/actions/variables)," and "[AUTOTITLE](/rest/deployments/branch-policies)."

Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.

## Using an environment

Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.

When a workflow references an environment, the environment will appear in the repository's deployments. For more information about viewing current and previous deployments, see "[AUTOTITLE](/actions/deployment/managing-your-deployments/viewing-deployment-history)."

{% data reusables.actions.environment-example %}

## Deleting an environment

{% data reusables.actions.permissions-statement-environment %}

Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.actions.sidebar-environment %}
1. Next to the environment that you want to delete, click {% octicon "trash" aria-label="Delete environment" %}.
1. Click **I understand, delete this environment**.

You can also delete environments through the REST API. For more information, see "[AUTOTITLE](/rest/repos#environments)."

## How environments relate to deployments

{% data reusables.actions.environment-deployment-event %}

You can access these objects through the REST API or GraphQL API. You can also subscribe to these webhook events. For more information, see "[AUTOTITLE](/rest/repos#deployments)," "[AUTOTITLE](/graphql/reference/objects#deployment)" (GraphQL API), or "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads#deployment)."

## Next steps

{% data variables.product.prodname_actions %} provides several features for managing your deployments. For more information, see "[AUTOTITLE](/actions/deployment/about-deployments/deploying-with-github-actions)."
Thank you for sharing your online asset with our team workspace for our nonprofit organization website page and site affiliate stems online business sites programs development director layouts for our business website url https and http ID multiple level digital site domain web portal database access point by Google Domain Sites Enterprise of view interface proxy server legacy with Google Business Manager Suite and Google Global maps Proxy Home and special Mediant extinct format digital partnership, Affiliate Marketing applications and services branding promotional content for the public network and multiple companies digital YouTube,Need for Good, Black American Computer Science & Technology analysts Engineering services Beta Developer Business AI Executive AI Executive director layouts and education services for Technology analysts Program Manager Association Manager Advanced marketing services Agency Al Gemini digital legion solutions services for community console and accreditions-campaign services Elite's Expertise Technology analysis data documention official prospects associated registered business platform creatively forum connected with targeted data entry editor for productions to industry support for Google Advertisement Company active legacy APIs Group for Google Global communications projects formation for assessment materials and services grant program engineering youth education projects with resources relations Google database coding Expert and Impact Internal Federal Online Safety forums Interpol EU digital data protections business online affiliate, US Government Historianal Engineer/ GU federal credit historianal engineer authorizations data ancestor publish media operational status Creative Workshop Research Educator/publications of federal laws and development director of digital declaration of operating title developer/media reporter freelancer/Digital media layouts Editors library of admissions resources to document CFR social auditor/Legal Congressional library NIST- Publication editor/author network stems/IRS legacy API proper connected to published work flow data for documentions and publication governmental education business programs/OSHA office operations education business services operations legal program Manager Education business website affiliate marketing branding institutionalized workshop Administrative role in ID access member and ID me digital programs affiliate online network Digital primary contact of admin Publication authorized to provide assistance assessment and support products editor enforcement to provisions of support and services for execution to directory layouts and quote information for publication listings optional-way for educational solutions for business accounts for IDme.gov/Congressional-library .gov and digital CFR.gov publication listings for documention data entry workload and digital support input for provisions forms for Google data questionnaire and email assistance logistics and 
partnership with logistics taskforce advocate & member of investors to investigation on edition Mediant extinct format diverse director and discussion regarding console quote for various resoureful commissions sites business resolution to public website user, business organization and individual requested assistance with resources and information for usage status on legacy data updates forms on the public website regarding review & question for answers to solutions topics for website help pages with support assistant requests. Our business website page and online business are innovative and creatively forum connected to assistance individuals with what ever they may need help with bith on and off line as well as an active member of multiple interactions Digital Health and assistance works for our assistance with the Google brand name for our own itemized form of public assistance and services for this primary caring organization that's fully invested in various types of business affiliated networks/programs / partner/ branding promotional content digital declaration social media creations/affiliate online business marketing/education services and supporting editing skills to learn customized and data entry for different types of community supportive systems tools-Productions-Make alliances all-types of business associate with resources support for any platform or operator division eaudience in input assessment solutions Creative workshop business services sales consultant in the constructively directed streams of public information to help with the the security data and judgement and support assistance with online knowledge of maintaining the proper connected dedications formation for everyone to digital obtain the sublevels of information and services understand and direction and communication accredited knowledge and circumstances educational solutions for their requests to be addressed in a respectful manner and equally with the proper connected information to the public resources and architecture inquire if they're needed understanding of accessibility to reflect, adamantly fix the problem of what ever the quarry was in the constructively directed streams of course offer to exactly direct interaction with the proper instructions for their requests based upon the proper dedications formation reference to the public information to be administrator on behalf of the digital business level particulars while additional access to update internal requirements details assignment direct deposit install instructions for the public data documention and the entitled format of the inquiry output for the publicated directory services methods to provide help with assurance of whatever corporate clarification to be used for the reasonable assetment of input that's would be needed for that individualized record of requestsending unusual form for the most recent/accord information to be delivered for that precise and targeted questions to the proper connected provisions with in the constructively correctiodesigned material for asset management of misrepresented overall consistency reputation for the companies integrity, safety, and legitimate policy's and general guidelines of the digital business online regulated language and procalls to be upheld in regards to their own requirements for this online business website page digital declaration invitation for the next step of course the addressing the proper infornative about input to available to choose the most effective beneficently revision term of agency aggressive interest in obtaining the best opportunity for investing the public with the multi factor investing knowledge for the duty infinite information and services site fix with resources to two or more option available to choose from this position offers the customer to be able to create an secondary opinion on different methods to try to be able to gain control and access on trying to accord complete whatever issues that maybe an issued response to the file transfer resources to provide help with the errors progressive data entry inquiries to this documented incident report for the unlimited resources to information rely required to commotions accreditions-campaign really reputable to the public question for assistance with resources by the intended source link and data accommodations for the proposal application services trouble shooting resolved encounter with the reliable programs data connection with donations inquiry to be answered with the device instead step in calculated reliability means for the solutions to the topic of candidates console services unconditionally contact of interest of content containing solutions that are attached to and relevant to the question of the topic of each individual request for digital assistance with resources requests for the most recent a certain quality dedicated rebutionals to production and services includion data recovery information to deverepeate/offer platforms advice for the total reference to help execute and supportive measures for optimum instruction of accurate understanding step by step with the instructions in language that innovatively understandable to the average individual's for the results information layouts in a full clear and content to betterment that most people can combative comprehend the digital resources instructive formation method for everyday adults with little of no interest in learning computer laggo or logistics writings for comparison of repost ID reactivate compatible computer science technology position instructions vs regular function format language detail on how to use or fix errors and computer updates for delivery reposition to accommodate a really teachable upstanding to help with the desired format to offer fully data and easily flow written individuals information for a more reliable user experience with resources and wanted to share interest succession of gainful interactions, and more direct hand on usages for that and more data products than can be produced and purchased by the intended cusmoter for the extra revolution for the public health od public perception of this primary company active in business sales and longevity status with the proper connected dedications for the understanding of accessibility to reflect materials that the average person can understand and use affirmatively daily or either assessment of assets to provide assistance with needed resources for the addition public prosecutor to be conserved effect on efforts to make life decisions for a easier relationship with building the public knowledge of useful content and self services understanding of accessibility to reflect on the values of that rebrand realistic resolution for the proper connected educations formation of the digital material and essential facts about self service support for future reference to help with error fixes that gainful profitable meaningful Internally investment in regards to resourcful methods to provide assistance with the proper easy understand reasonable assets for the self confidence in be able to make adjustments for the individualize ability to provide self Care Provider for ones self by providing the proper multipurpose operational services steps in regards to this one 
request for digital programs assistance with the device and digital programs products that are directional compatible with the proper steps by step instructions and in the information landscape of language that's clear and precise to accurately address concerns the proper regards request of assistance with or either about a products or services request for digital programs development director layouts and education for instructions on how to officially operate the proper the digital products that maybe needed at that point for resources and details instructions assessments issues. This organization has always had the best and most meaningful interest in privileged in regards to the public delivery of service data accessment in regards to assisting individuals with the proper connected educations formation for individuals to become properly informated with the necessties tools, data, institutional related instructions, transactional status recording details, trouble shooting methods to respectfully and written in regards to underable language steps to the provider proper understandable response to the public logistics of the contractly regarding methods of direct data recovery and usage instructions for with an update most openings for incorprative formcreatively directoryhandout for director full compatible comprehension for required edition information to upon the public requested for real situation inquiry to provision's of addressing the proper dedications formation for the issues of connection questions and answers of assistance with resources for references needed to be completed by development director assistance developers for Google blogs and community services representative that are available to help with any type of additionalitemized litemized for asset management of material on the public website that are available to help with individuals related in the constructively directed work flow assessment of the digital produces and services protocols for assistance with resources to provisions of items problems with the proper connected safety and policies for associated historically corporate clarification policies and rules of the digital platform for the users regulated language in the constructively correctiodesigned material form of with the decorative of the digital business online website and developer freelance platforms regulated language and data support systems tools-Productions-Make alliances all-types of concerns to the business request formats and editing delivery design database affidavit admissions to the primary care providers mastering estimated assistance with the device for the time of the demographic that's related to the public register for handling the digital programs development editor and educational solutions workspaces/entitled digital programs fields of admissions to this incorporation affiliated workspace title of the digital coding and encompass business programmer management reserve required networks dedications of reasonables /market place in regards to Google platform creative Merchant  online business sites Collaboration Account Manager workshop Administrative connected with targeted Digital Primary programmers scholarship to provisions by Microsoft Data Business Relations Marketing LLC online business sites space for Entity and financial service memberships. Dedications of admissions to Harmony Blue Branding Organization Business Group for Google industries & affiliate online services. This organization assistment digital declaration communities in the constructively directed proper composition of the digital business online website work-flow information.The professionalism was directory targeted focused groups. Aligned for our relatives platform and the whole founded objective as an financial needed for govnen revenue goal for Supportive interest and equipment services for the needed resources and services fees goal of the digital business financially funding ongoing income for the public emerged infrastructure security both digitally and pumpkin to be provided effective benefits of ensured for the public sector that every single young individuals has been prevented with the proper connected dedications formation for the entire consult of the digital programs development and educational services, data access to cover the wireless work location, the proper number of devices and equipment for business services and supporting resourceful supplies for operations legal rights for the next few months of digital Business of more digital devices ti the property location and service representative office operations manager to help with growth and support several types of resoureful products and digital products software for future contact servers modeled to provide assistance with the best opportunity for the young individuals to succeed and mobility follow through the internal templates of information without being offset with digital logically issues, digital proforms database speed for a crosspoint platform router with resources to support a of 20 services device service all as once.The desire digital devices that have an operational status title to fighting history of becoming jetlack and development issues with the performance connection issues of jinky data access on site uploads to slow down after an few weeks even with resources to provide assistance for consideration time duration to resl time to automatic logout reload to the home website work-flow format even with the proper connected dedications formation of self proved to model to reset  focused hardware for the public safety of privacy data properly connected with the best suitable amount of firewall services equipment. poorly performing computers and devices still are known to becoming problematic with an short span of time with the daily  used forum and including the recognized digital entry of technology projects and designs for anyone who is using the property directly from the tribe source related fields of unlimited uploading and downloading materials that are associated with the device drive root redirect to the function settings and this is so even with resources of saving the proper connected resources to an extent thumb drive as well. I've been extremely fortunate to receive offers for products at a discount business price but the conditions based on the price of the bulk product did not meet and will not meet my expectations including them being refurbished it's also additional Factor business conditions of course of this months of this primary needed contributions and resources entity properties for development director layouts and education services for the public youth program reliability means for the results of the same the primary consideration of the digital business level of connection functionality of the digital business online demand security equipmentinterest in provisions of comparisons services for the public online business official prospects production of the Harmony Blue Branding LLC online business sites space in the incorporate status of the Publication records accredited nonprofit organization for the Harmony Blue, the digital business connection to synced detail to corporate clarification to the public forum for the Harmony Blue Foundation and unique classification for the Harmony House Foundation assets provide assistance with resources of additional details reference to reflect the public executive operations legal certification federal credit union of the Harmony Blue index as an multiple increments of the digital business online website work-flow merger page for qualified directly exempt status with the proper dedications standards of admissions resources this nonprofit organization legal verification records forms on network with the Secretary of state certified business office operations manager Owner Founder Cheyanna Henry and solely partnership with public records and license documentions records for sole business claiming to and for legalize owner of public reconciliation in the official prospects of associated registered business with both federal and state judication for legal administrative corporate direct access deposit data approval for operations status with the United States registered for legal operations for business services and operations services of corporate clarification business government office for registration regulations required to commotions accreditions-campaign with the BBB and certification business verification of public records by Yelp and Google Business operations with good legal rights to active declaration of operating title of Administrator grade A+ standings. Legacy financial ownership over all of alliance titles that are attached to the public business services operations legal rights to law federal government services based upon regulations required the proper connected position with resources and public data access records for linked to the business process records forms on relisted and financial filing requirements needed for this primary position to be with in the constructively directed format of the digital business online state of justify jurisdiction records forms on iexceptions provisions with no notarized expiration date of service for social revision to be ablity or applied for this primary business organization and business design database Group for prevention of the digital programs and architecture health to services associated registered to and publication owner transfer design  trademarked to be delivered in relation to this corporative and incorporation business organization services.All services supportive and professional submission for the publication inquiries for and about this and any other details information on operational status and digital declaration library of services.Are on my website work-flow merger page for qualified directly to the public records forms on networking and content communication accredited to this incorporation affiliated workspace title Account Manager and services for the public enlightened to this incorporation digital programs and development projects, foundation services and supporting resoureful health services and operations management community project design database information for usage status on legacy APIs Group chat or organization fundraising publication of the digital business online information for services. Reminder to be available for future reference to this incorporation business online information would be dedications and migration file folders to the url website work-flow merger pages of this primary position in caring with resources to nonprofit organization for the public view of the digital progressive information to be attached to the public historical website work-flow dashboard proxy server Domain and listed as correspondent for the public records forms legacy knowledge page for qualified directly on the values proper connected to the public dedications formation for the online businesses.All legal certification for the public libraries of interest in various types of products and digital services for the business response to this incorporation affiliated workspace title are on the public library url website website pages for this proxy servers. Observation about the benefitical of the digital business online website work-flow and business organization services mergers to the business development resources to nonprofit organization for the public development resources to provide assistance with the public this encoded within this website sites drive folders and digital historianal engineer authorizations in regards to declaration date and data entries to provided effective benefits are in the work spaces for this primary website primer proxy server legacy APIs website. All division of course audience of all three years of services are incorporated into the digital programs website digital pages of this primary URL multi-level link for this online campaign analytical publications as high 87% know legally operations of course of 2 or more years of services and operations legal rights to owner of office annual budgets for this business and finance services related information for the public resources records reflect materials that are attached and completed recorded for the business cab be found on this business data digital proxy server legacy APIs https openings and http openings and the whole website platform workspaces for historical content for visual recognition of the public library of admissions to various types of resoureful community projections to campaign analytical virtual public market contact in regards to revenue agency Dedications of admissions to various project design database information for this primary program to the business operations of publication of services and supporting resoureful health and youth community project and community effort to assistance with the proper dedications formation of the financial support and diversity development director layouts and education services for the public youth community services projects analytics promotional content ideas on custom crafts resources to nonprofit organization and business management program engineering department promoter and the whole website in traditional company active legacy APIs program engineering services for claiming methods to respectfully writing of during of works for grant writing projections for grant funding proposal abd application listed dayes of applied for real literacy commissions prospects production of abilities for achievvmultiple documents grant funding for services assistant director resources to help with nonprofit organization needs and resources requirements for professional provisions items for comparison of quality assistance to thrive property management preparation for proposals require to apply for existing administrators economical successfully following platform creatively forum connected synced targeted Digital Health assistant associate with resources to nonprofit organization and business foundation services for the public needs to accommodate the proper connected resources to provide help with growth of the companies abilities for the original particulars network online goals in achieved affection for the fundraising publication financial support of expansion efforts for resources to help more individuals, the proper functions and digital support for future devices and services equipment needed to be able to meet the public needs for access to relevant access to multiple dynamic digital programs and urgent support products to the web portal database information for usage of official prospects in regards to the business response for the children of the local communities for the device assistance with optional costs for the equipment and office operations supplies and offices functional company desktop office operations legal solutions products including the publication business services office furnitures for the proper connected dedications formaled item's request to provide the proper access to the business network digital programs for the public education programs development director of digital programs and support development resources for the proper dedicated hopeful abilities for achievements are the best opportunity to gainful commending for our youths and family services programs. Due to si many unrealistic situations in and with this apmis-step on this business data access to existing administrator assistance with the proper regards to the prixy servers legacy APIs access point of services and operations output linked database for the business page for this primary website always degraging to regularly payloads server Domain IP links to the business director digital business online website connected repetatively resource communication projections to request fix fork and code maintenance for the public url lodge on the public library database information to register domain sites and hist submit support site location for this online business to be always conversing to having static data issues daily with without any type of additionalitemized litemized reasons for this online related to repeatly be happening so frequently between every 6 to 9 months reaction time during the time of the very needed resources submission to application process for financial assistance request for business services operations resources funding grant submittions for the business needs of assistance with resources to the non-profit organization primary assistance with resources to help with principles projects.Digital issues surrounding this primary problem resourced in the self eduational knowledge of the digital respectful needs to be ables to essential expert knowledge and analysis skills to learn every single team member learning how to prioritize the proper digital programs development education business services operations specialist and sales Special edition for the public sector in regards to conpter science technology engineering and engineering logistics software management skills for the custom crafted resources to assist with the proper dedications provisions of advanced releases to products exist respect for developmentprerelease beta affiliate corporate sponsorship executive director layouts and education business management support systems tools-Productions-Make alliances openings for branding commission analysis company market for exterior demolition of primary test kit before release date of service software and Digital website work-flow connection with top category criteria corporation with in the technology fiejds and the top confirmed development software and firmware company market logistics testing performance pre-release for prompt entity properties trailer to use for limited capacity for testing of their system products before the publication launch date. I have the best experience with the most of thud typr of offers to from this organization collected collaborative branding promotional content program edition and merging with the faculty team contributions accessibly of the digital programs and signature corporations like IBM, Digital icon Douglas navg. software for mathematics and digital device technology and development, software engineer, authorizations data T-Mobile Wireless  Opera, Microsoft Data content Business workshop Administrative corporate sponsorship program for various resources and grant business website resources, Google docs data access to studio sponsorship for YouTube creative publication contract for Google industries,etc...the list extinct is too long to be able to review alliance various types of products affiliated with the proper regards to special digital distribution for the best opportunity for this online business website services to required online business sites and data access for the program execution to directory layouts itemized formal exceptions provisions items as resources network logistics to requested resources to nonprofit organization and business organization services development teams to provide assistance with the unlimited and excellent customer relations professionals dream of being able to achieve the vision obtained operations over the course of this business operations legacy of business associate director of education services for the public education business management relationships in regards to to active Dedications leadership and learning resources for expanding knowledge of maintaining itemized formal exceptions provisions of comparisons services representative of tenant executive director digital programs development education skills essential on the public value existing administrators economically successfully as well the proper related features are needed for the digital programs and achievements directly to depositing input and output data website work-flow information for future configuration and executive director design database coding Expert services for claiming methods engineering reliable programs safe for folk and report various types of rep repositories of data matrix and external factors platforms of operational status on puls format director layouts in Editorial direct access to multiple resources to reverse and revise coding error for fix assistance solutions about benefitical test switch to provide help with sight solutions of innovation directory layouts itemized reference format dymierrors category individualized recording on the values proper connected dedications formation for the public estimated assistance in the constructively streams in regards to the business response to the plugs and the drive in public library of course the title verify auto correct information for usage status on the public health administration services request for digital programs development assistant Lead to promotion self support products for seeking various edit ms data portal program engineering and workload source link acodes for the necessary details tgat ate needed to accommodate the digital coding trouble shooting content for overall consistency ratio of an authorized execution of yermsk pass through documents shell and the whole needs website fault resources runs to provide the proper connected plug ti to be attached to the public library or file information for usage status correct designed to provide direct dig requirements to the intended rebutionals errors shorten and runs to the pull test for the next debug submit to the input genuine supportive code sites link support products for the next and steps in regards to services true and finally pass transportation to the point of request for digital programs come gainful profitable meaningful Internally to be effective to help with support products for web portal Repo in regards to the website resource link to seek for historical reviews real time health to this work flow data documention and refigured changes to the outline output of the digital programs development of the foundation of relationship knowledge of the digital programs and availability for obtaining additional information and educational services in with the current specialisted field of study in computer science and digital science technology analysis of to respectfully accomplish the proper regards to complete call my ab respectable engineer and at least obtain the ultra development skills for custom crafts and forever evolving tr trending fields in regards to digital programs and safety data analytics experience with the proper regards to tine and structure study in the duration or probably 3more to 5 more years plus recent My authorizations data certificate for the relation to the understanding of every single available resources learning programs and educational skills for custom crafts incentives of one day becoming a real estate agent with the proper dedications certifications and licences  required to be completely honored to be considered as an experienced digital engineering and professional programs development in software design and support business management skill publication connected to the website renewal portal workspace title database coding effectively meet the recognitions requirements needed to obtain the ultra high quality dedicated for everyone to be able to obtain.The  team and me the principal and somewhat their platform teacher and motivation support for them to learn and own the ability to build up the proper infrastructure understanding with the best opportunity to reach this reliability programs goal needed to be conserved effective for asset with resources and data entry Firm in the constructively directed streams of capable abilities to provide assistance with the proper environment to assist with financial support, education editing writing experience with digital programs networks for social media marketing executive operations legion solutions services technology project design database coding system with development publication listings recognized skills set based upon the public team learning group editing videos and exercises for this primary position to hopefully get better and understanding of accessibility to reflect material for various types of business administration resources to nonprofit organization and business organization services assistance representative of corporate operations manager to help access with candidates to acknowledge that required pre-purchased studies experience to lecture with the device tools with Microsoft Data content forms to create an connection updated skills to custom crafts and received the proper dedications formation for obtaining quality team contributions accessibly in every single person ability to test for the public titles and recipient joint submission application for the next step in service requirements for this primary term in regards to owner coverage for the publication testing process for recipient estimated purchasing for each team member to be able to obtain their own personal licenses for the required field and experiences of study as listings for receiving the public records forms for benefitical certification business license titled of course studies course associated registered business Digital computer science technology development of corporate clarification professional provisions for themselves and executive directly to their own personal employment resume for the achievement official prospective titled associated registered business services operations The dedication of value in legal rights to be able to gain the proper certification in business and finance services sales Special Mediant list form of certified title, professional associated degree in business management of legal data recording, direct access to multiple resources online to verification of products regulations corporate network registration for audit digital programs development in virtual license business services operations and tax experience finance soes list extinct with the IRS letter offical approval for this online legacy of business associate course. For nonprofit organization members that are associated with this organization works and daily practices.This programs for architecture require to the public as an optional need education business to help with growth and development succeed offer succession of gainful estimated knowledge for the online class entrepreneurs and certification tools for tax accountant in legal rights to business workers in operational statues of becoming a real licensed tax advocate professional associated with the United States supportive assistance with resources to help with education business management of the operator of the foundation financial advisor and resources to provide internal revenue agency assistance with an internal security data filing taxes for the public citizens of America and the tax paying company that are in need of help with processing and preparing tax records online. When the public and legal forms position to receive connected commentary compensation for office operations legal filing assistance with resources for references. All services provider for the proper connected costs for helping team members the digital testing fees. This organization has been through so many different types of extremely struggling and economic setbacks and imagined data access challenging for this company first 2 years of services. As an understanding and relevant development heartbreaking situations for the public sector in the past and a bunch of other side issues that are presented to the business still to this date. We're outstandingly advocating for and on behalf of anyone who has been extremely dedicated to help with the volunteer services and operations support for the public community services payment from the public are just a well acknowledged Thank you for being their for the connection hands on work and within the public district ministry works with us and in commitment and time to help with making successful decisions on small community services projects and assisting us by recite the public community about our relationship knowledge and communication skills for creating ideas for public relations to help with touching the proper people who have and have been in or are actively going through homeless and other financial difficulties or essential facts experts dur to health care and documented psychological issues. Our primary organization services targeted construction missionaries of services and extremely dire  awareness custom growth downtown areas changes to the public library of admissions resources to educational solutions adviser and services graduated in the constructively corrections designed course materials that related to passed the digital certificate testing.This online experiences with the trust federal revenue agencies in the US educational programs, and online curriculums to the  educational solutions for this one or two listed website public courses. 
rses. 
educational solutions for this one or two listed website public courses. 
rses. 
