---
title: "Template Anatomy"
date: 2019-10-28T11:11:22Z
weight: 100
---

This section of the workshop builds upon the knowledge in the [CloudFormation Fundamentals section](../30-cloudformation-fundamentals).

The following example CloudFormation template is in YAML format and shows the overall structure of a template and its sections.

```yaml
  AWSTemplateFormatVersion: version date  # Recommended

  Description: Description of the template  # Recommended

  Transform: A string or a map of template transforms  # Optional
    
  Metadata:  # Optional
    Map: of
    Template: metadata
    
  Parameters:  # Optional
    Map: of
    Template: parameters
      
  Mappings:  # Optional
    Map: of
    Template: values

  Conditions:  # Optional
    Map: of
    Template: conditions
    
  Resources:  # Required
    Map: of
    Template: resources
    
  Outputs:  # Optional
    Map: of
    Template: outputs
```

The only *required* element of a template is **Resources**.

The elements *can* be in any order, but as your templates become larger and more complex, it can be helpful to use the logical order shown above.

Most CloudFormation templates you encounter (such as the example templates in the [AWS CloudFormation Sample Templates GitHub repository](https://github.com/awslabs/aws-cloudformation-templates)) will be laid out in a similar format to the above example.
