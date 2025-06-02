# Using SignPath with GitHub Actions or Azure DevOps

This project demonstrates signing artifacts using [SignPath](https://about.signpath.io) from GitHub Actions workflows or Azure DevOps pipelines.

## GitHub

Signing is invoked in the `sign` step of [.github/workflows/build-and-sign.yml](.github/workflows/build-and-sign.yml). 

See [the documentation](https://about.signpath.io/documentation/trusted-build-systems/github) for a full documentation of the integration.

## Azure DevOps

Signing is invoked in the `Sign` step of [azure-pipelines.yml](azure-pipelines.yml).

See [the documentation](https://about.signpath.io/documentation/trusted-build-systems/azure-devops) for a full documentation of the integration.

## Policy demonstrations

This project demonstrates the following attempts to violate SignPath policies and how they are averted on the control plane:

* This step selects the appropriate [signing policy] depending on the branch name. The actual branch must match the branch condition of the selected signing policy. The [`attempt-signing-release`] branch demonstrates how SignPath will detect incorrect attempts.
* The [`release/malicious-dll`] branch demonstrates how SignPath will detect content-level violations of the [artifact configuration].

## Configuration

To use this demo with your own SignPath subscription, you need to get access to SignPath's GitHub Actions preview. Please contact support@signpath.io.

* Fork this repository
  * Uncheck _Copy the main branch only_
* In your SignPath organization, create a project with 
  * Slug: `Demo_Application` 
  * Repository URLs: Your forked GitHub repository, e.g. `https://github.com/my/github-actions-demo`
  * Trusted Build Systems: Link _GitHub.com_ or _AzureDevOps.com_ respectively
  * Add the following artifact configuration as default: [.signpath/artifact-configurations/default.xml](.signpath/artifact-configurations/default.xml)
  * Add a `test-signing` signing policy
  * Add a `release-signing` signing policy with origin verification enabled and restricted to `main` and `release/*` branches

### GitHub configuration

* Create an [API token] in SignPath and add it as a GitHub Actions secret `SIGNPATH_API_TOKEN` (make sure the user is a submitter in your signing policies)
* Add your SignPath _Organization ID_ as a GitHub Actions variable `SIGNPATH_ORGANIZATION_ID` (click your organization's name at the upper right corner)
* Enable Actions for your GitHub repository

### Azure DevOps configuration

* Install the extension and add a service connection to your project


[signing policy]: https://about.signpath.io/documentation/projects#signing-policies
[artifact configuration]: https://about.signpath.io/documentation/projects#artifact-configurations
[`attempt-signing-release`]: https://github.com/SignPath/github-actions-demo/blob/feature/attempt-signing-release/.github/workflows/build-and-sign.yml#L46
[`release/malicious-dll`]: https://github.com/SignPath/github-actions-demo/blob/release/malicious-dll/src/Build.ps1#L4

[API token]: https://about.signpath.io/documentation/users#interactive-api-token
