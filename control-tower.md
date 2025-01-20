# AWS Control Tower

Quick and easy setup for multi-account environment. Orchestrates AWS services, such as Organizations, Cloudwatch, IAM Identity Center, Cloudformation, etc. to simplify setup.

## Features
1. **Landing Zone**: Multi-account environment management
- Enables SSO/ID Federation through IAM Identity Center (f/k/a AWS SSO)
- Centralizes logging and auditing through Cloudwatch, Cloudtrail, AWS Config, and SNS

2. **Guard Rails**: Detects and mandates rules/standards across all accounts in a landing zone

3. **Account Factory**: Automates and standardizes new account creation beyond what AWS Organization offers.

4. **Dashboard**: Single page oversight of an organization


## Architecture
A Control Tower is created within an AWS account. The AWS account becomes the **management account** for the landing zone. The control tower uses other AWS services to provide its feature set.

- AWS Organizations enables multi-account structure, OUs, and SCPs. Control Tower creates two OUs when initially set up.
    - A **foundational OU** named `security`.
        - Two accounts named, `Audit Account` and `Log Archive Account`, are created in the foundational OU. 
        - The `Audit Account` is for users who need access to Control Tower logs. It can be used for any third-party tool that needs to audit the environment. For example, SNS might be used for alerts on changes to governance policies and and Cloudwatch for landing zone wide metrics.
        - The `Log Archive Account` is for users that need access to all login information for all accounts across the landing zone, such as AWS Config and Cloudtrail logs. Provides secure **read-only** access; requires granting explicit access to the account.
    - A **custom OU** named `sandbox`
- IAM Identity Center (AWS SSO) enables login for internal identities or federated identities
- AWS Account Factory enables creation of AWS accounts in an automated manner. Control Tower uses Cloudformation under the hood for creating stacks and AWS Config and SCPs to detect/prevent drifts from policies.  

**Home Region**: Control Tower is housed in the region where it is initially deployed.