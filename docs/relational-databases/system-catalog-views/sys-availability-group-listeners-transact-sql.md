---
title: sys.availability_group_listeners (TRANSACT-SQL) |Microsoft 文档
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- availability_group_listeners_TSQL
- sys.availability_group_listeners
- sys.availability_group_listeners_TSQL
- availability_group_listeners
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_group_listeners catalog view
- Availability Groups [SQL Server], listeners
ms.assetid: b5e7d1fb-3ffb-4767-8135-604c575016b1
caps.latest.revision: 19
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 3ce9f05f546fe9ad9a1d22d17424fe8f06482499
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/04/2018
---
# <a name="sysavailabilitygrouplisteners-transact-sql"></a>sys.availability_group_listeners (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  对于每个 Always On 可用性组中，将返回零行，该值指示没有网络名称与可用性组中，关联，或在 Windows Server 故障转移群集 (WSFC) 将返回每个可用性组侦听器配置行群集。 此视图显示从群集中收集的实时配置。  
  
> [!NOTE]  
>  此目录视图不说明 WSFC 群集中定义的 IP 配置的详细信息。  
  
|列名|数据类型|Description|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|可用性组 ID (**group_id**) 从[sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)。|  
|**listener_id**|**nvarchar(36)**|群集资源 ID 的 GUID。|  
|**dns_name**|**nvarchar(63)**|可用性组侦听器的已配置网络名称（主机名）。|  
|**port**|**int**|为可用性组侦听器配置的 TCP 端口号。<br /><br /> NULL = 侦听器在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 外部配置，并且其端口号尚未添加到可用性组。 若要添加的修改 LISTENER 选项的端口，pleaseuse [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)]语句。|  
|**is_conformant**|**bit**|此 IP 配置是否符合标准，可为下列值之一：<br /><br /> 1 = 侦听器符合标准。 在其 Internet 协议 (IP) 地址之间仅存在“OR”关系。 *符合的*包含每个 IP 配置，由[CREATE AVAILABILITY GROUP](../../t-sql/statements/create-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)]语句。 此外，如果 IP 配置是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之外创建的（例如通过使用 WSFC 故障转移群集管理器），但可以通过 ALTER AVAILABILITY GROUP tsql 语句修改，该 IP 配置也符合标准。<br /><br /> 0 = 侦听器不符合标准。 通常，这表示无法通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 命令配置的 IP 地址可以直接在 WSFC 群集中定义。|  
|**ip_configuration_string_from_cluster**|**nvarchar(max)**|该侦听器的群集 IP 配置字符串（如果有）。 NULL = 侦听器没有虚拟 IP 地址。 例如：<br /><br /> IPv4 地址：`65.55.39.10`。<br /><br /> IPv6 地址：`2001::4898:23:1002:20f:1fff:feff:b3a3`|  
  
## <a name="security"></a>Security  
  
### <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [监视可用性组 & #40;Transact SQL & #41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
