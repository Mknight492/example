# Current behaviour

I recently came across an issue where I configured Renovate to make all PR with all "minor" and "patch" updates.

This worked as expected, however I also then wanted to filter the PR to only packages which have a major version >1.0.0 so that they were lower risk package updates.

This works as expected for new PR but I don't believe it's working work existing PRs event though the PR summary states it's filtered out the non major version >1.0.0 version.

I first configured renovate to merge all minor and patch packages like so:

``` json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "enabled": true,
  "packageRules": [
    {
      "groupName": "All non-major go dependencies",
      "matchUpdateTypes": ["minor", "patch"],
    }
  ]
}
```

Which raised the correct PR updating the packages like so:

github.com/aws/aws-sdk-go-v2 v1.16.1 -> github.com/aws/aws-sdk-go-v2 v1.18.0
github.com/satori/go.uuid v1.1.0 -> github.com/satori/go.uuid v1.2.0
golang.org/x/tools v0.7.0 -> golang.org/x/tools v0.8.0

However after updating the renovate.json to exclude the package with an unstable API:

``` json 
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "enabled": true,
  "packageRules": [
    {
      "groupName": "All non-major go dependencies",
      "matchUpdateTypes": ["minor", "patch"],
      "matchCurrentVersion": ">=1.0.0"
    }
  ]
}
```

the PR claims that it's no-longer updating the golang.org/x/tools package

Yet it's still include in the PR here

## Expected Behaviour

After updating the renovate configuration it would update the PR to exclude the packages updates which should be filtered out based on the new packageRules.
