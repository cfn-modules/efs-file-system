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
        BackupRetentionPeriod: '30' # optional
        BackupScheduleExpression: 'cron(0 5 ? * * *)' # optional
      TemplateURL: './node_modules/@cfn-modules/efs-file-system/module.yml'
```

## Examples

* [ec2-efs](https://github.com/cfn-modules/docs/tree/master/examples/ec2-efs)

## Related modules

* [asg-singleton-amazon-linux2](https://github.com/cfn-modules/asg-singleton-amazon-linux2)
* [ec2-instance-amazon-linux](https://github.com/cfn-modules/ec2-instance-amazon-linux)
* [ec2-instance-amazon-linux2](https://github.com/cfn-modules/ec2-instance-amazon-linux2)
* [ebs-volume](https://github.com/cfn-modules/ebs-volume)

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
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> where traffic is allowed from on port 2049 to the filesystem</td>
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
    <tr>
      <td>BackupRetentionPeriod</td>
      <td>The number of days to keep backups of the EFS file system (set to 0 to disable)</td>
      <td>30</td>
      <td>no></td>
      <td>[0-35]</td>
    </tr>
    <tr>
      <td>BackupScheduleExpression</td>
      <td>A CRON expression specifying when AWS Backup initiates a backup job</td>
      <td>cron(0 5 ? * * *)</td>
      <td>no></td>
      <td></td>
    </tr>
  </tbody>
</table>
