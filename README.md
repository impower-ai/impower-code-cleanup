# Usage

## 1. Create a new Workflow

Create a new file in the repository under the following path:

 ```.github/workflows/example.yml```

## 2.1 Copy Template A (without Repository Checks):
```
# This workflow will run the Impower.AI code cleanup using the Impower Code Standards (https://github.com/impower-ai/impower-code-standards)

name: ReSharper Code Cleanup using Impower Code Standards

on:
  pull_request:
    types: [ closed ]
    branches: [ "main" ]

jobs:
  impower-code-cleanup:
    uses: impower-ai/impower-code-cleanup/.github/workflows/impower-code-cleanup.yml@main
    with:
      auto_merge: true
      #nuget_sources: '["https://your-server.com/index.json", "https://your-other-server.com/index.json"]'
    secrets:
      PAT: ${{ secrets.STANDARDS_PAT }}
```
# or
## 2.2 Copy Template B (with Repository Checks):
```
# This workflow will run the Impower.AI code cleanup using the Impower Code Standards (https://github.com/impower-ai/impower-code-standards) 
while also performing various checks on the target repository/branch

name: ReSharper Code Cleanup using Impower Code Standards

on:
  pull_request:
    types: [ closed ]
    branches: [ "main" ]

jobs:
  impower-repository-checks:
    uses: impower-ai/impower-code-cleanup/.github/workflows/impower-repository-checks.yml@main
    with:
      #fail_on_unprotected: true
      branch: "main"
    secrets:
      PAT: ${{ secrets.STANDARDS_PAT }}

  impower-code-cleanup:
    needs: impower-repository-checks
    uses: impower-ai/impower-code-cleanup/.github/workflows/impower-code-cleanup.yml@main
    with:
      auto_merge: true
      #nuget_sources: '["https://your-server.com/index.json", "https://your-other-server.com/index.json"]'
    secrets:
      PAT: ${{ secrets.STANDARDS_PAT }}
```

## 3. Adjust inputs & secrets

### With Repository Checks

```fail_on_unprotected```
**Optional**  (default: **false**)  
```true``` If the branch is not protected, the workflow fails and stops the chain.  
```false``` Branch Protection is ignored and the chain continues.  

```branch```
**Required**  
```string``` The name of the branch to check for Branch Protection Rules.

### Without Repository Checks

```auto_merge```
**Required**  
```true``` The resulting PR will automatically be merged and the temporary branch deleted.  
```false``` Do not merge automatically (the PR has to be reviewed manually).  


```PAT```
**Required**  
A personal access token that has permissions to clone the standards repository and create the pull request along with its branch.  


````nuget_sources````
**Optional**  
Pass any NuGet package source URLs as a JSON string array.  
You do not have to pass this value if you're not using any additional NuGet sources.

<br/><br/>
## Note:
The default branch naming conventions have changed:  
You might have to change the branch for the activation from "```main```" to "```master```" if your repository was created a while ago.