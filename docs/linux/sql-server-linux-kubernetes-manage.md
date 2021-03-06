---
title: Manage a SQL Server Always On availability group Kubernetes
description: This article explains how to manage a SQL Server Always On Availability Group in Kubernetes.
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.custom: "sql-linux"
ms.technology: linux
monikerRange: ">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
---
# Manage SQL Server Always On Availability Group Kubernetes

To manage an Always On Availability Group on Kubernetes, create a manifest and apply it to the cluster. The manifest is a `.yaml` file.  

The examples in this article apply to all Kubernetes cluster. The scenarios in these examples are applied against a cluster on Azure Kubernetes Service.

See an example of the complete deployment in [Always On availability groups for SQL Server containers](sql-server-ag-kubernetes.md).

## Fail over - SQL Server availability group on Kubernetes

To fail over an availability group primary replica to a different node in Kubernetes, use a job. This article identifies the environment variables for this job.

The following manifest file describes a job to manually fail over an availability group. 

Copy the contents of the example into a new file called `failover.yaml`.

[failover.yaml](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/high%20availability/Kubernetes/sample-deployment-script/templates/failover.yaml)

To deploy the job, use `Kubectl`.

```azurecli
kubectl apply -f failover.yaml
```

After you apply manifest file, Kubernetes runs the job. The job makes the supervisor elect a new leader and moves the primary replica to the SQL Server instance of the leader.

After you run the job, delete it. The job object in Kubernetes stays after completion so you can view its status. You need to manually delete old jobs after noting their status. Deleting the job also deletes the Kubernetes logs. If you don't delete the job, future failover jobs will fail unless you change the job name and the pod selector. For more information, see [Jobs - Run to Completion](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/).

## Rotate credentials

Rotate the credentials to update the SA and the master key.

Copy the [rotate-creds.yaml](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/high%20availability/Kubernetes/sample-deployment-script) locally use `kubectl` to apply it to your cluster.

```azurecli
kubectl apply -f rotate-creds.yaml
```

## Next steps

[Access the Kubernetes dashboard with Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/kubernetes-dashboard)

[SQL Server availability group on Kubernetes cluster](sql-server-ag-kubernetes.md)
