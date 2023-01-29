# Aikido Security Github action

This repository contains an application that can be used in Github action workflows. It will trigger a scan in Aikido to make sure no new critical issues are introduced into your application. This action is available in any paid plan at Aikido.

## Using the action

This is an example workflow you could use to trigger a scan each time changes are pushed to your main branch: 

```yaml
name: My Github action
on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Detect new vulnerabilities
        uses: AikidoSec/github-actions-worfkflow
        with:
            secret-key: ${{ secrets.AIKIDO_SECRET_KEY }}
```

The action has 3 possible outcomes: 
- 'SUCCEEDED': the scan was completed successfully and we did not encounter any new critical issues
- 'FAILED': the scan was completed successfully, but we found new critical issues
- 'TIMED_OUT': the scan did not complete before the set timeout. In this case we won't let the action fail, but we do return this special case to not block your pipeline.

By default, the action fails if the scan did not complete within 2 minutes. You can control this behaviour by setting the `fail-on-timeout` parameter to false for the action, like so: 

```yaml
name: My Github action
on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Detect new vulnerabilities
        uses: AikidoSec/github-actions-worfkflow
        with:
            secret-key: ${{ secrets.AIKIDO_SECRET_KEY }}
            fail-on-timeout: false
```

Now the action will still shut down after 2 minutes, but it won't fail and block your pipeline.

## Contributing

Install the dependencies  
```bash
$ npm install
```

When the changes have been implemented, you need to build and package the code for release. Run the following commands and commit it to the repository.
```bash
$ npm run build && npm run package
```

## Change action.yml

The action.yml defines the inputs and output of our action.

See the [documentation](https://help.github.com/en/articles/metadata-syntax-for-github-actions)
