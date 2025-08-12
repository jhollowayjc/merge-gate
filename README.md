# merge-gate

This repository contains a very basic merge-gate control in the form of a GitHub Action.

## how it works

The action exposed in this repository, when invoked, will check `gate.yaml` to see if the following condition exists:

`global_freeze` is `true` or the calling repository is found in the lists `frozen_repositories`. 
If either of these conditions are `true`, we exit with `exitcode 1`.


Demo Repositories:
- https://github.com/jhollowayjc/merge-gate-demo.git
- https://github.com/jhollowayjc/merge-gate-demo-frozen.git


When combined with a ruleset (example below), we can enforce the check passes

```json
{
  "name": "Merge Gate",
  "target": "branch",
  "source_type": "Repository",
  "enforcement": "active",
  "conditions": {
    "ref_name": {
      "include": [
        "~DEFAULT_BRANCH"
      ]
    }
  },
  "rules": [
    {
      "type": "required_status_checks",
      "parameters": {
        "strict_required_status_checks_policy": true,
        "do_not_enforce_on_create": false,
        "required_status_checks": [
          {
            "context": "Merge Gate Check",
            "integration_id": 15368
          }
        ]
      }
    }
  ],
  "bypass_actors": []
}
```
