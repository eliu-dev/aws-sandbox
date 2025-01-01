# Cloudformation

Allows creating and deleting resources using YAML or JSON templates.

## Structure (Overview)
- `AWSTemplateVersionFormat` specifies the AWS
- `Resources (required)` lists the set of resources for Cloudformation to create
- `Description (optional)` is a free text field. If both an `AWSTemplateFormatVersion` and a `description` are included, the `description` must immediately follow `AWSTemplateFormatVersion` (*exam point*).
- `Metadata (optional)` controls how cloudformation presents the template in its UI
- `Parameters (optional)` supports adding fields to prompt the user for more data (e.g., machine size, availability zones, etc.). You can use controlled fields and have default values.
- `Mappings (optional)` supports lookup tables for automation such as mapping image names to test and prod environments
- `Conditions (optional)` supports conditional logic through a two-step process: (1) creating a condition and (2) using the condition in the template.
Example: Create a condition that is true if an environment `parameter` is set to "production". Use the condition in the `resources` section for creating production resources.
- `Outputs (optional)` once the template is finished, the outputs determine what cloudformation outputs for reporting purposes 


```YAML
AWSTemplateFormatVersion: "version date"

Description: 
    String

Metadata:
    template metadata

Parameters:
    set of parameters

Mappings:
    set of mappings

Conditions:
    set of conditions

Transform:
    set of transforms

Resources:
    set of resources

Outputs:
    set of outputs

```

## Structure (Resources)
The template effectively contains resources and everything else.

### Logical Resources
Resources inside a template are called **logical resources** and each logical resource is called an **instance**. Each instance has a **type** (e.g., EC2) and **properties** (details about the instance).

### Stacks
Cloudformation creates a **stack** when it receives a template. **A template can create multiple stacks.**

Deleting a stack deletes all resources within it.

### Physical Resources
For every logical resource (instance), Cloudflormation will create a corresponding **physical resource** (e.g., an actual EC2 instance).

## Deployment
When a file is uploaded to Cloudformation, it stores it to a S3 bucket that it creates (prefixed by cf).

### Parameters
- `LatestAmiId` allows you to specify which image to use. You can default images to the latest version (e.g., using `/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64`)
- `SSHandWebLocation` allow you to specify which IP address range for accessing a machine (if using EC2)

### Outputs
- The `!Ref` keyword allows you to reference parameters and the **default attribute** of a resource in the template that is referenced by its logical ID
- The `GetAtt` keyword allows you to select which attribute to use from a reference resource