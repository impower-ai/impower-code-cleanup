# Usage

## 1. Create a new Workflow

Create a new file in the repository under the following path:

 ```.github/workflows/example.yml```

## 2. Copy Template:
```
# This workflow will run the Impower.AI code cleanup using the Impower Code Standards defined in https://github.com/impower-ai/impower-code-standards

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

## 3. Adjust inputs & secrets

```auto_merge```
**Required**  
```true``` the resulting PR will automatically be merged and the temporarily created branch deleted.  
```false``` do not merge automatically (the PR has to be reviewed manually).  


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