# Cookiecutter to scaffold a c7n-left policy structure

This cookiecutter generates the shell of a [c7n-left](https://github.com/cloud-custodian/cloud-custodian/tree/main/tools/c7n_left) policy.

## Usage

[Install Cookiecutter](https://cookiecutter.readthedocs.io/en/stable/installation.html) and run:

```console
cookiecutter -fso <policy root directory> gh:ajkerrigan/cookiecutter-c7n-left
```

**Note:** Why `-fso`?
- `-f / --overwrite-if-exists`: lets us add to existing directories
- `-s / --skip-if-file-exists`: ...but doesn't let us overwrite _files_
- `-o / --output-dir`: tells the cookiecutter where to place policy-related files
## More Information

This cookiecutter prompts for a few key fields:

- Provider
- Service
- Resource type
- Policy short name
- Policy full name

For a policy enforcing HTTPS in a CloudFront distribution, the responses might look like this:

- Provider: aws
- Service: cloudfront
- Resource type: cloudfront (defaults to <service>)
- Policy short name: enforce-https
- Policy full name: aws-cloudfront-enforce-https (defaults to <provider>-<resource type>-<policy short name>)

Which would generate a file tree like the following under <policy_root_directory>:

```
/<policy root directory>/
├── aws
│   ├── cloudfront.yaml
│   └── tests
│       ├── aws-cloudfront-enforce-https
│       │   ├── left.plan.yaml
│       │   ├── match1.tf
│       │   └── nonmatch1.tf
```

## Next Steps

This produces only the skeleton of a policy. From there the next steps are:

- Define Terraform snippets for matching and non-matching resources
    - Inside `.tf` files under `<provider>/tests/<policy>`
- Define policies in `<provider>/<service>.yaml`
- Define expected matches inside `<provider>/tests/<policy>/left.plan.yaml`
    - This cookiecutter seeds `left.plan.yaml` with an empty list.
    - With no expectations in `left.plan.yaml`, the output of `c7n-left test <provider>` should include all policy matches against your Terraform snippets. That output can be used to populate `left.plan.yaml`.
