[![Build Status](https://travis-ci.org/cfn-modules/efs-file-system.svg?branch=master)](https://travis-ci.org/cfn-modules/efs-file-system)
[![NPM version](https://img.shields.io/npm/v/@cfn-modules/efs-file-system.svg)](https://www.npmjs.com/package/@cfn-modules/efs-file-system)

# cfn-modules: AWS EFS file system

AWS EFS file system with [alerting](https://www.npmjs.com/package/@cfn-modules/alerting).

## Install

> Install [Node.js and npm](https://nodejs.org/) first!

```
npm i @cfn-modules/efs-file-system
```

## Usage

> By default, the EFS file system is only [writable by the Linux root user](https://docs.aws.amazon.com/efs/latest/ug/accessing-fs-nfs-permissions.html).

```
---
AWSTemplateFormatVersion: '2010-09-09'
Description: 'cfn-modules example'
Resources:
  FileSystem:
    Type: 'AWS::CloudFormation::Stack'
    Properties:
      Parameters:
        VpcModule: !GetAtt 'Vpc.Outputs.StackName' # required
        ClientSgModule: !GetAtt 'ClientSg.Outputs.StackName' # required
        AlertingModule: !GetAtt 'Alerting.Outputs.StackName' # optional
        KmsKeyModule: !GetAtt 'Key.Outputs.StackName' # optional
        PerformanceMode: generalPurpose # optional
        NumberOfAvailabilityZones: !GetAtt 'Vpc.Outputs.NumberOfAvailabilityZones' # optional (must match with the value of the vpc module)
      TemplateURL: './node_modules/@cfn-modules/efs-file-system/module.yml'
```

## Parameters

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Description</th>
      <th>Default</th>
      <th>Required?</th>
      <th>Allowed values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>VpcModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/vpc">vpc module</a></td>
      <td></td>
      <td>yes</td>
      <td></td>
    </tr>
    <tr>
      <td>ClientSgModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> where traffic is allowed from on port 5432 to the database</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>AlertingModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/alerting">alerting module</a></td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>KmsKeyModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/kms-key">kms-key module</a></td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>PerformanceMode</td>
      <td>The performance mode of the file system</td>
      <td>generalPurpose</td>
      <td>no</td>
      <td>[generalPurpose, maxIO]</td>
    </tr>
    <tr>
      <td>NumberOfAvailabilityZones</td>
      <td>How many availability zones should be used? Same as in the vpc module!</td>
      <td>3</td>
      <td>no></td>
      <td>[2-3]</td>
    </tr>
  </tbody>
</table>

## Limitations

* Secure: EFS file system is not backed up
