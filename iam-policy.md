# Identity Policies (IAM Policies)
Identity policies are a set of security statements that grants or denies access to AWS services and features to the identities that use the policy.

Created in JSON.

## Statements
When an identity attempts to access an AWS resource, it needs to prove its identity (**authentication**). After authentication, the relevant statements, one or more, are applied to the identity.

1. **Sid (optional)**: Recommended field for identifying what the statement does.

2. **Action (required)**: Identifies the services and operations on it that are allowed. Formatted as an array of `"service": "operation"` key-value pairs. Actions can be specified in three ways: (i) individual actions, (ii) a list of actions, or (iii) wildcard actions using `*`.

3. **Resources (required)**: Identifies the individual resources covered by the statement. Resources can be specified in three ways: (i) individual resources, (ii) a list of resources, or (iii) wildcard resources using `*`.

4. **Effect (required)**: For statements where the action and resource match the identity's request, the effect whether AWS grants access, `Allow`, or denies access, `Deny`. Resources are identified by their `ARN` (amazon resource number).

```json
"Version": "2012-10-17",
"Statement": [
    {
        "Sid": "FullAccess",
        "Action": ["s3:*"],
        "Effect": "Allow",
        "Resource": "*"
    },
    {
        "Sid": "DenyCatBucket",
        "Action": ["s3":"*"],
        "Effect": "Deny",
        "Resource": ["arn:aws:s3:::catgifs", "arn:aws:s3::::catgifs/*"]
    }
]
```

### Multiple Policy Sources
Multiple policy sources can govern a user's access. The policies are aggregated and evaluated together to determine if they have access to the resource.

Example: When a user attempts to access a resource, their access can be governed by (1) an identity policy applied to their user, (2) an identity policy applied to a group they are a part of, or (3) an identity policy applied to the resource. 

## Overlapping Statements

1. Explicit `Deny` statements takes effect over all overlapping statements. It wins even if there is a conflicting `Allow`.
2. Explicit `Allow` statements take effect unless there is a conflicting `Deny` statement.
3. Implicit `Deny` takes effect for any resources that are not covered by any explicit `Deny` or `Allow` statements.

## Formats

Policies can be written as **inline policies** and **managed policies**.


Inline policies are policies that are created for each relevant user account. For example, if 3 team members need the same policy, an inline policy is created for each of them. Updating the policy requires updating the inline policy on each of their accounts. This approach is only recommended for exceptions because it does not scale.

Managed policies are policies that are created once and attached to each user. It should be used for normal default policies.