---
title: "分布式可用性组 (SQL Server) | Microsoft Docs"
ms.custom: 
ms.date: 06/20/2017
ms.prod: sql-server-2016
ms.reviewer: 
ms.suite: 
ms.technology:
- dbe-high-availability
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- Availability Groups [SQL Server], distributed
ms.assetid: 
caps.latest.revision: 
author: MikeRayMSFT
ms.author: mikeray
manager: jhubbard
ms.translationtype: HT
ms.sourcegitcommit: 01f20dd99963b0bb1be86ddc3e173aef6fb3e8b3
ms.openlocfilehash: e390d6efa26dcb628da636bc9bcf7c8fac54af65
ms.contentlocale: zh-cn
ms.lasthandoff: 08/11/2017

---
# <a name="distributed-availability-groups"></a>分布式可用性组

分布式可用性组是 SQL Server 2016 中引入的一种新功能，作为现有 Always On 可用性组功能的一种变体。 本文阐明了分布式可用性组的某些特性，并对现有 [SQL Server 文档](https://docs.microsoft.com/en-us/sql/sql-server/sql-server-technical-documentation)进行了补充。

> [!NOTE]
> “DAG”不是 Distributed Availability Group（分布式可用性组）的正式缩写，因为此缩写已用于 Exchange Database Availability Group（数据库可用性组）功能。 此 Exchange 功能与 SQL Server 可用性组或分布式可用性组无关。

若要配置分布式可用性组，请参阅[配置分布式可用性组](configure-distributed-availability-groups.md)。

## <a name="understand-distributed-availability-groups"></a>了解分布式可用性组

分布式可用性组是一种特殊类型的可用性组，它跨两个单独的可用性组。 基础可用性组配置在两个不同的 Windows Server 故障转移群集 (WSFC) 群集上。 加入分布式可用性组的可用性组无需处于同一位置。 它们可以是物理也可以是虚拟的，可以在本地、公有云中或支持可用性组部署的任何位置。 只要两个可用性组可以进行通信，就可以使用它们配置分布式可用性组。

传统的可用性组在 WSFC 群集中配置资源。 分布式可用性组不会在 WSFC 群集中配置任何内容。 有关它的所有内容都保留在 SQL Server 中。 若要了解如何查看分布式可用性组的信息，请参阅[查看分布式可用性组信息](#viewing-distributed-availability-group-information)。 

分布式可用性组要求基础可用性组具有侦听器。 在创建分布式可用性组时，通过 ENDPOINT_URL 参数为其指定已配置的侦听器，而不是像传统可用性组那样，为独立实例提供基础服务器名称（若是 SQL Server 故障转移群集实例 [FCI]，则提供与网络名称资源相关联的值）。 尽管分布式可用性组的每个基础可用性组都具有侦听器，但分布式可用性组不具有侦听器。

下图显示了分布式可用性组的高级视图，该分布式可用性组跨两个可用性组（AG 1 和 AG 2），这两个可用性组配置在各自的 WSFC 群集上。 分布式可用性组总共有四个副本，每个可用性组中两个。 每个可用性组可以支持最大副本数，因此，基于标准版的分布式可用性组最多可以具有 4 个副本，而基于企业版的分布式可用性组最多可以具有 18 个副本。

<a name="fig1"></a> ![分布式可用性组的高级视图][1]

可以在分布式可用性组中将数据移动配置为同步或异步。 但是，与传统可用性组相比，分布式可用性组中的数据移动略有不同。 尽管每个可用性组都具有主要副本，但只有一个数据库副本加入可以接受插入、更新和删除的分布式可用性组。 如下图所示，AG 1 为主要可用性组。 其主要副本将事务发送到 AG 1 的次要副本和 AG 2 的主要副本。 然后，AG 2 的主要副本将更新 AG 2 的次要副本。 


![分布式可用性组及其数据移动][2]

使 AG 2 的主要副本接受插入、更新和删除的唯一方法是从 AG 1 手动故障转移分布式可用性组。 在前面的图中，因为 AG 1 包含数据库的可写入副本，所以发出故障转移将使 AG 2 成为可以处理插入、更新和删除的可用性组。 有关如何将一个分布式可用性组故障转移到另一个分布式可用性组的信息，请参阅[故障转移到次要可用性组]( https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/distributed-availability-groups-always-on-availability-groups)。

> [!NOTE]
> SQL Server 2016 中的分布式可用性组支持通过使用选项 FORCE_FAILOVER_ALLOW_DATA_LOSS 仅从一个可用性组故障转移到另一个可用性组。

## <a name="sql-server-version-and-edition-requirements-for-distributed-availability-groups"></a>分布式可用性组的 SQL Server 版本要求

分布式可用性组当前仅适用于通过相同的主要 SQL Server 版本创建的可用性组。 例如，当前必须使用 SQL Server 2016 创建加入分布式可用性组的所有可用性组。 因为 SQL Server 2012 或 2014 中不存在分布式可用性组功能，因此通过这些版本创建的可用性组不能加入分布式可用性组。 

> [!NOTE]
> 可通过标准版或企业版配置分布式可用性组，但不支持在分布式可用性组中混用这两个版本。

因为存在两个单独的可用性组，因此在加入分布式可用性组的副本上安装服务包或累积更新的过程与传统可用性组略有不同：

1. 通过更新分布式可用性组中第二个可用性组的副本开始此操作。

2. 修补分布式可用性组中主要可用性组的副本。

3. 与标准可用性组一样，将主要可用性组故障转移到其副本（而不是次要可用性组的主要副本），并对其进行修补。 如果除主要可用性组外不存在副本，则需要手动故障转移到第二个可用性组。

> [!NOTE]
> 对于以后的 SQL Server 版本是否允许以前的版本加入同一分布式可用性组，目前尚无通知。 如果启用这一方案，分布式可用性组将加入 SQL Server 版本升级计划。

### <a name="windows-server-versions-and-distributed-availability-groups"></a>Windows Server 版本和分布式可用性组

分布式可用性组跨多个可用性组，每个可用性组分别位于其自己的基础 WSFC 群集上，分布式可用性组是仅限 SQL Server 的构造。  这意味着容纳各个可用性组的 WSFC 群集可以具有不同主版本的 Windows Server。 如前一部分中所述，SQL Server 的主版本必须相同。 与[初始图](#fig1)非常相似，下图显示 AG 1 和 AG 2 加入分布式可用性组，但每个 WSFC 群集是不同版本的 Windows Server。


![WSFC 群集具有不同版本的 Windows Server 的分布式可用性组][3]

单个 WSFC 群集及其相应的可用性组遵循传统规则。 也就是说，它们可以加入域，也可以不加入域（Windows Server 2016 或更高版本）。 在单个分布式可用性组中合并两个不同可用性组时，存在四种情况：

* 两个 WSFC 群集加入同一域。
* 每个 WSFC 群集加入不同域。
* 一个 WSFC 群集加入域，一个 WSFC 群集不加入域。
* 两个 WSFC 群集都不加入到域。

如果两个 WSFC 群集加入同一域（不受信任的域），创建分布式可用性组时无需执行任何特殊操作。 对于未加入同一域的可用性组和 WSFC 群集，通过证书启用分布式可用性组，方法类似于为独立于域的可用性组创建可用性组。 若要查看如何配置分布式可用性组的证书，请遵循[创建独立于域的可用性组](domain-independent-availability-groups.md#create-a-domain-independent-availability-group)下的步骤 3-13。

对于分布式可用性组，每个基础可用性组中的主要副本必须具有彼此的证书。 如果已有未使用证书的终结点，请使用 [ALTER ENDPOINT](https://docs.microsoft.com/en-us/sql/t-sql/statements/alter-endpoint-transact-sql) 重新配置这些终结点，以反映证书的使用。

## <a name="distributed-availability-group-usage-scenarios"></a>分布式可用性组使用方案

下面是分布式可用性组的三个主要使用方案： 

* [灾难恢复和更加轻松的多站点配置](#disaster-recovery-and-multi-site-scenarios)
* [迁移到新硬件或配置，可能包括使用新硬件或更改基础操作系统](#migration-using-a-distributed-availability-group)
* [通过跨多个可用性组，将单个可用性组中的可读副本增加到 8 个以上](#scaling-out-readable-replicas-with-distributed-accessibility-groups)

### <a name="disaster-recovery-and-multi-site-scenarios"></a>灾难恢复和多站点方案

传统可用性组要求所有服务器属于同一 WSFC 群集，因此很难跨多个数据中心。 下图显示传统多站点可用性组体系结构，包括数据流。 有一主要副本，它将事务发送到所有次要副本。 与分布式可用性组相比，这种配置在某些方面不甚灵活。 例如，必须在 WSFC 群集中实现 Active Directory（如果适用）和仲裁见证等。 且可能还需要考虑 WSFC 群集的其他方面，如更改节点投票。

![传统多站点可用性组][4]

分布式可用性组可为跨多个数据中心的可用性组提供更加灵活的部署方案。 甚至可以在使用了[日志传送]( https://docs.microsoft.com/en-us/sql/database-engine/log-shipping/about-log-shipping-sql-server)等功能的环境中使用分布式可用性组。 但是，与传统可用性组不同，分布式可用性组不能包含事务延迟的应用程序。 这意味着，对于错误更新或删除数据的人为过失事件，可用性组或分布式可用性组无法提供帮助。

分布式可用性组是松散耦合的，在这种情况下，这意味着它们无需单个 WSFC 群集，并且它们由 SQL Server 维护。 由于 WSFC 群集是单独进行维护的，且最初在这两个可用性组之间异步同步，因此很容易在其他站点配置灾难恢复。 每个可用性组中的主要副本都同步其自己的次要副本。

* 分布式可用性组仅支持手动故障转移。 在切换数据中心的灾难恢复情况下，不应配置自动故障转移（除极少数例外）。 
* 很可能无需为多站点或子网 WSFC 群集设置某些传统项或参数（如 CrossSubnetThreshold），但仍需了解数据传输的另一层的网络延迟。 不同之处在于，每个 WSFC 群集都维护其自身的可用性；群集并非包含四个节点的大型实体。 如上图所示，实际有两个单独的双节点 WSFC 群集。  
* 建议采用异步数据移动，因为此方法将用于灾难恢复目的。
* 如果在主要副本和第二个可用性组的至少一个次要副本之间配置同步数据移动，并在分布式可用性组上配置同步移动，则分布式可用性组将等待，直到所有同步副本确认它们具有数据。

### <a name="migrate-by-using-a-distributed-availability-group"></a>使用分布式可用性组进行迁移

由于分布式可用性组支持两个完全不同的可用性组配置，因此它们使得灾难恢复、多站点方案和迁移方案更加容易。 无论是迁移到新的硬件还是虚拟机（本地或公有云中的 IaaS），只要过去可使用备份、复制和还原或日志传送的位置，配置分布式可用性组都允许其中发生迁移。 

在保留相同 SQL Server 版本时更改或升级基础操作系统，迁移功能尤其有用。 尽管 Windows Server 2016 允许在相同硬件上从 Windows Server 2012 R2 滚动升级，但大多数用户都选择部署新的硬件或虚拟机。 

若要完成迁移到新的配置，在此过程末尾，停止指向原始可用性组的所有数据流量，并将分布式可用性组更改为同步数据移动。 此操作可确保第二个可用性组的主要副本完全同步，以免出现数据丢失。 验证同步后，将分布式可用性组故障转移到[故障转移到次要可用性组](https://msdn.microsoft.com/en-US/library/mt651673.aspx)部分中的第二个可用性组。

迁移后，第二个可用性组成为了新的主要可用性组，可能需要执行以下任一操作：

* 重命名次要可用性组上的侦听器（可能删除或重命名原始主要可用性组中的一个旧侦听器），或通过原始主要可用性组中的侦听器重新创建一个侦听器，以便应用程序和用户可以访问新的配置。
* 如果无法进行重命名或重新创建，请将应用程序和用户指向第二个可用性组上的侦听器。

### <a name="scale-out-readable-replicas-with-distributed-availability-groups"></a>通过分布式可用性组扩大可读副本

单个分布式可用性组最多可拥有 16 个次要副本，具体视需要而定。 因此，它最多可拥有 18 个副本供读取，包括不同可用性组的两个主要副本。 此方法意味着，多个站点可实现近实时访问，以便向各应用程序报告。

与仅使用单个可用性组相比，分布式可用性组可帮助扩大只读场。 分布式可用性组可通过两种方式扩大可读副本：

* 可使用分布式可用性组中第二个可用性组的主要副本来创建另一个分布式可用性组，即使数据库未处于恢复状态。
* 还可以使用第一个可用性组的主要副本来创建另一个分布式可用性组。

换句话说，就是主要副本可以参与两个不同的分布式可用性组。 下图显示 AG 1 和 AG 2 均参与分布式 AG 1，而 AG 2 和 AG 3 参与分布式 AG 2。 AG 2 的主要副本是分布式 AG 1 的次要副本和分布式 AG 2 的主要副本。

![通过分布式可用性组扩大读取][5]

下图显示 AG 1 为两个不同分布式可用性组的主要副本：分布式 AG 1（包含 AG 1 和 AG 2）和分布式 AG 2（包含 AG 1 和 AG 3）。


![使用分布式可用性组扩大读取的另一示例][6]


在前面两个示例中，三个可用性组中至多可以具有 27 个副本，这些副本可用于只读查询。 

[只读路由]( https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server)当前不适用于分布式可用性组。 使用侦听器进行连接的所有查询都将转到主要副本。 否则，将需要配置每个副本，以允许将所有连接作为次要副本并直接访问它们。 在 SQL Server 2016 的更新或将来的 SQL Server 版本中，此行为可能会发生更改。

## <a name="initialize-secondary-availability-groups-in-a-distributed-availability-group"></a>初始化分布式可用性组中的次要可用性组

分布式可用性组将[自动种子设定]( https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group)设计为用于初始化第二个可用性组上主要副本的主要方法。 如果执行以下操作，将可在第二个可用性组的主要副本上进行完整的数据库还原：

1. 通过 WITH NORECOVERY 还原数据库备份。
2. 如有必要，通过 WITH NORECOVERY 还原适当的事务日志备份。
3. 创建第二个可用性组，而不指定数据库名称，将 SEEDING_MODE 设置为 AUTOMATIC。
4. 使用自动种子设定创建分布式可用性组。

在将第二个可用性组的主要副本添加到分布式可用性组时，将根据第一个可用性组的主数据库检查副本，且种子设定会将数据库捕获到源。 以下是一些注意事项：

* 第二个可用性组的主要副本上的 `sys.dm_hadr_automatic_seeding` 中所示的输出将显示 `current_state` 失败，原因是“种子设定检查消息超时”。

* 第二个可用性组的主要副本上的当前 SQL Server 日志将显示种子设定已生效，且 [LSN]( https://docs.microsoft.com/en-us/sql/relational-databases/sql-server-transaction-log-architecture-and-management-guide) 已同步。

* 第一个可用性组的主要副本上的 `sys.dm_hadr_automatic_seeding` 中所示的输出将显示 COMPLETED 的 current_state。 

* 种子设定还可与分布式可用性组具有不同行为。 为在第二个副本上开始进行种子设定，必须在副本上发出命令 `ALTER AVAILABILITY GROUP [AGName] GRANT CREATE ANY DATABASE` 命令。 尽管对于参与基础可用性组的任何次要副本来说，这种情况仍然适用，但第二个可用性组的主要副本已具有正确权限，允许在将种子设定添加到分布式可用性组后开始进行种子设定。 

### <a name="view-distributed-availability-group-information"></a>查看分布式可用性组信息 
    
如前所述，分布式可用性组是仅限 SQL Server 的构造，未见于基础 WSFC 群集。 下图显示了两个不同的 WSFC 群集（CLUSTER_A 和 CLUSTER_B），每个群集都有其自己的可用性组。 此处仅讨论 CLUSTER_A 中的 AG1 和 CLUSTER_B 中的 AG2。 

<!-- ![Two WSFC clusters with multiple availability groups through PowerShell Get-ClusterGroup command][7]  -->
<a name="fig7"></a>
```
PS C:\> Get-ClusterGroup -Cluster CLUSTER_A

Name                            OwnerNode             State
----                            ---------             -----
AG1                             DENNIS                Online Available Storage               GLEN                  Offline Cluster Group                   JY                    Online New_RoR                         DENNIS                Online Old_RoR                         DENNIS                Online SeedingAG                       DENNIS                Online


PS C:\> Get-ClusterGroup -Cluster CLUSTER_B

Name                            OwnerNode             State
----                            ---------             -----
AG2                             TOMMY                 Online Available Storage               JC                    Offline Cluster Group                   JC                    Online
```

All detailed information about a distributed availability group is in SQL Server, specifically in the availability-group dynamic management views. Currently, the only information shown in SQL Server Management Studio for a distributed availability group is on the primary replica for the availability groups. As shown in the following figure, under the Availability Groups folder, SQL Server Management Studio shows that there is a distributed availability group. The figure shows AG1 as a primary replica for an individual availability group that's local to that instance, not for a distributed availability group.

![View in SQL Server Management Studio of the primary replica on the first WSFC cluster of a distributed availability group][8]


However, if you right-click the distributed availability group, no options are available (see the following figure), and the expanded Availability Databases, Availability Group Listeners, and Availability Replicas folders are all empty. SQL Server Management Studio 16 displays this result, but it might change in a future version of SQL Server Management Studio.

![No options available for action][9]

As shown in the following figure, secondary replicas show nothing in SQL Server Management Studio related to the distributed availability group. These availability group names map to the roles shown in the previous [CLUSTER_A WSFC cluster](#fig7) image.

![View in SQL Server Management Studio of a secondary replica][10]

The same concepts hold true when you use the dynamic management views. By using the following query, you can see all the availability groups (regular and distributed) and the nodes participating in them. This result is displayed only if you query the primary replica in one of the WSFC clusters that are participating in the distributed availability group. There is a new column in the dynamic management view `sys.availability_groups` named `is_distributed`, which is 1 when the availability group is a distributed availability group. To see this column:

```
SELECT ag.[name] as 'AG Name', ag.Is_Distributed, ar.replica_server_name as 'Replica Name' FROM    sys.availability_groups ag, sys.availability_replicas ar       
WHERE   ag.group_id = ar.group_id
```

An example of output from the second WSFC cluster that's participating in a distributed availability group is shown in the following figure. SPAG1 is composed of two replicas: DENNIS and JY. However, the distributed availability group named SPDistAG has the names of the two participating availability groups (SPAG1 and SPAG2) rather than the names of the instances, as with a traditional availability group. 

![Example output of the preceding query][11]

In SQL Server Management Studio, any status shown on the Dashboard and other areas are for local synchronization only within that availability group. To display the health of a distributed availability group, query the dynamic management views. The following example query extends and refines the previous query:

```
SELECT ag.[name] as 'AG Name', ag.is_distributed, ar.replica_server_name as 'Underlying AG', ars.role_desc as 'Role', ars.synchronization_health_desc as 'Sync Status' FROM    sys.availability_groups ag, sys.availability_replicas ar,       
sys.dm_hadr_availability_replica_states ars       
WHERE   ar.replica_id = ars.replica_id and     ag.group_id = ar.group_id and ag.is_distributed = 1
```
       
       
![Distributed availability group status][12]


To further extend the previous query, you can also see the underlying performance via the dynamic management views by adding in `sys.dm_hadr_database_replicas_states`. The dynamic management view currently stores information about the second availability group only. The following example query, run on the primary availability group, produces the sample output shown below:

```
SELECT ag.[name] as 'Distributed AG Name', ar.replica_server_name as 'Underlying AG', dbs.[name] as 'DB', ars.role_desc as 'Role', drs.synchronization_health_desc as 'Sync Status', drs.log_send_queue_size, drs.log_send_rate, drs.redo_queue_size, drs.redo_rate FROM    sys.databases dbs, sys.availability_groups ag, sys.availablity_replicas ar, sys.dm_hadr_availability_replica_states ars, sys.dm_hadr_database_replica_states drs WHERE   drs.group_id = ag.group_id and ar.replica_id = ars.replica_id and ars.replica_id = drs.replica_id and dbs.database_id = drs.database_id and ag.is_distributed = 1
```

![Performance information for a distributed availability group][13]


### Next steps 

* [Use the availability group wizard (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

* [Use the new availability group dialog box (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
* [Create an availability group with Transact-SQL](create-an-availability-group-transact-sql.md)

This content was written by [Allan Hirt](http://mvp.microsoft.com/en-us/PublicProfile/4025254?fullName=Allan%20Hirt), Microsoft Most Valued Professional.

<!--Image references-->
[1]: ./media/dag-01-high-level-view-distributed-ag.png
[2]: ./media/dag-02-distributed-ag-data-movement.png
[3]: ./media/dag-03-distributed-ags-wsfcs-different-versions-windows-server.png
[4]: ./media/dag-04-traditional-multi-site-ag.png
[5]: ./media/dag-05-scaling-out-reads-with-distributed-ags.png
[6]: ./media/dag-06-another-scaling-out-reads-using-distributed-ags-example.png
[7]: ./media/dag-07-two-wsfcs-multiple-ags-through-get-clustergroup-command.png
[8]: ./media/dag-08-view-smss-primary-replica-first-wsfc-distributed-ag.png
[9]: ./media/dag-09-no-options-available-action.png
[10]: ./media/dag-10-view-ssms-secondary-replica.png
[11]: ./media/dag-11-example-output-of-query-above.png
[12]: ./media/dag-12-distributed-ag-status.png
[13]: ./media/dag-13-performance-information-distributed-ag.png

 
