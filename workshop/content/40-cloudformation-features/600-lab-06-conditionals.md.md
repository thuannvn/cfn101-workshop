---
title: "Lab 06: Conditionals"
date: 2019-11-12T14:15:05Z
weight: 600
---


## Introduction

In the goal of making CloudFormation template flexible and reusable, it would be useful to create certain resources only sometimes. For example, in dev and test environments, you may not wish to create a CloudFront distribution.

Conditions are a way to achieve this. Conditions define a boolean value that evaluate to `true` or `false`. This condition can then be referenced by components of `Resources` and `Outputs` sections of a CloudFormation template.

Let's take a look at an example.

### Example

```yaml
Parameters:
  Environment:
    Description: "Environment type of the stack"
    Default: "Dev"
    AllowValues:
      - "Dev"
      - "Production"

Conditions:
  # IsProduction is True if the Environment parameter is 'Production',
  # else it is false
  IsProduction: 
    !Equals [ !Ref Environment, "Production" ]

  # IsNotProduction is False if the condition 'IsProduction' is True,
  # else it is True
  IsNotProduction: 
    !Not [Condition IsProduction]
  

Resources:
  EC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: "t3.micro"
  
  ApplicationLoadBalancer:
    Type: AWS::EC2::ElasticLoadBalancingV2

    # Reference the CreateLoadBalancerCondition
    Condition: IsProduction
    Properties:
      Type: application

Outputs:
  LoadBalancerEndpoint:
    # Create Output only if IsProduction is true
    Condition: IsProduction
    Value:
      !GetAtt ApplicationLoadBalancer.DNSName
  EC2:
    # Create Output only if IsNotProduction is true
    Condition: IsNotProduction
    Value:
      !GetAtt EC2.PublicDnsName
    
```

## Conditionals Section

```yaml
Conditions:
  # IsProduction is True if the Environment parameter is 'Production',
  # else it is false
  IsProduction: 
    !Equals [ !Ref Environment, "Production" ]

  # IsNotProduction is False if the condition 'IsProduction' is True,
  # else it is True
  IsNotProduction: 
    !Not [Condition IsProduction]
```

In this template, two conditions are defined in the Conditions section.
The first is `IsProduction`. This will evaluate to `true` if the Parameter `Environment` is equal to `Production`. Else `IsProduction` will evalute to `false`. `IsProduction can be used to selectively create resources and outputs for the Production environment.

The second condtion is `IsNotProduction`. This will evaluate to the opposite of `IsProduction`. This can be used to create resources and outputs for non production environments.

## Resource Configuration

```yaml
Resources:
  EC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: "t3.micro"
  
  ApplicationLoadBalancer:
    Type: AWS::EC2::ElasticLoadBalancingV2

    # Reference the CreateLoadBalancerCondition
    Condition: IsProduction
    Properties:
      Type: application
```

Two resources are defined in the Resources section. The first is an EC2 instance. The second is an Application Load Balancer. The Application Load Balancer has a `Condition` key. The value of the `Condition` key is `IsProduction`. This resource will only be created when the value of the Condition is `true`. In this example, it will only be created when `IsProduction` is true.

## Output Configuration

```yaml
Outputs:
  LoadBalancerEndpoint:
    # Create Output only if IsProduction is true
    Condition: IsProduction
    Value:
      !GetAtt ApplicationLoadBalancer.DNSName
  EC2:
    # Create Output only if IsNotProduction is true
    Condition: IsNotProduction
    Value:
      !GetAtt EC2.PublicDnsName
```


Two outputs are defined in the template. They provide an endpoint URL. In this example, we want the Application Load Balancer endpoint if it exists. Otherwise, the EC2 Public DNS name should be provided. Each output has a Condition key included, referencing the two Conditions, `IsProduction` and `IsNotProduction` defined earlier in the template.


## Exercise

* Use existing template
* Add a new parameter, condition and resource

## Conclusion

Conditions can be a powerful, especially when used in combination with Parameters. Conditions can be used on Resources and Outputs to be created when all the conditions are met. 
