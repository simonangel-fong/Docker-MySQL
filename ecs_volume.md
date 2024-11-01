# ECS - Volume

- [ECS - Volume](#ecs---volume)
  - [](#)
  - [Create file system](#create-file-system)
  - [Add role](#add-role)
  - [Add storage to task definition](#add-storage-to-task-definition)

---

##

- Security Group to allow NFS

- Inbound:
  - Type: NFS
  - port range: 2049
  - source: default sg
- outbound rules
  - all traffic

---

## Create file system

- Mount target:
  - select security groups
- policy:

  - `Prevent root access by default*`
  - `Enforce in-transit encryption for all clients`

- Create access points
  - name
  - Root direcory path: `/ecs-ap`
  - POSIX user
    - User ID:1001
    - Group ID: 1001
  - Root directory creation permissions
    - Owner user ID:1001
    - Owner group ID:1001
    - Access point permissions: 0755

---

## Add role

- IAM:Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": [
        "elasticfilesystem:ClientMount",
        "elasticfilesystem:ClientWrite"
      ],
      "Resource": "arn:aws:elasticfilesystem:us-east-1:099139718958:file-system/fs-08ec7a150eb5e5c01",
      "Condition": {
        "ArnEquals": {
          "elasticfilesystem:AccessPointArn": "arn:aws:elasticfilesystem:us-east-1:099139718958:access-point/fsap-02bb36a2dce578e1a"
        }
      }
    }
  ]
}
```

- IAM:Roles
  - create role
    - Use case : `Elastic Container Service Task`
    - policy: above

---

## Add storage to task definition

- Docker image for demo:
  - `coderaiser/cloudcmd`

- In ECS: create task def
  - Task role: attach created role
  - storage: volumes
    - Volume type: EFS
    - File system ID
    - Access point
    - Advanced configurations
      - Transit encryption: on
    - Container mount points
      - Container
      - Source volume
      - Container path: `/var/lib/mysql`

- Create task in cluster
  - 
