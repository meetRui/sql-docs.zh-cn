---
title: 管理 Oracle CDC 服务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- createSrv
ms.assetid: 5972cee3-b1a9-4c56-aed6-bdddf84af283
caps.latest.revision: 8
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 64c1702e49bf68edd9e5b3abf316ea9289d9a6ce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="manage-an-oracle-cdc-service"></a>Manage an Oracle CDC Service
  您可以使用 CDC 服务配置控制台管理特定的 CDC 服务。  
  
 **选择要使用的 CDC 服务**  
  
1.  从 CDC 服务配置控制台的左侧窗格中，展开 **“本地 CDC 服务”**。  
  
2.  选择要使用的 CDC 服务。  
  
     也可以右键单击要使用的 CDC 服务，然后选择所需操作。 请参阅 [What can you do with a CDC Service](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md#BKMK_WhatcandowithCDCService)。  
  
 **OR**  
  
1.  从 CDC 服务配置控制台的左侧窗格中选择 **“本地 CDC 服务”** 。  
  
2.  从 CDC 服务配置控制台的中间部分，选择您要使用的服务。  
  
     也可以右键单击要使用的 CDC 服务，然后选择所需操作。 请参阅 [What can you do with a CDC Service](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md#BKMK_WhatcandowithCDCService)。  
  
##  <a name="BKMK_WhatcandowithCDCService"></a> What can you do with a CDC Service  
 您可在使用 CDC 服务时执行以下操作。  
  
### <a name="delete-the-service"></a>删除服务  
 从 CDC 服务配置控制台右侧的 **“操作”** 窗格，单击 **“删除”** 以便删除服务。  
  
 也可以右键单击要删除的 CDC 服务，然后选择“删除”。  
  
 **注意**：如果在删除服务时该服务正在运行，则该服务将在被删除前停止。  
  
 若要删除 Oracle CDC Windows 服务定义，程序需要对关联的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的 MSXDBCDC 数据库具有更新访问权限。 在您单击“确定”删除该服务后，程序将尝试在 MSXDBCDC 数据库中删除 Oracle CDC 服务注册。 如果程序由于没有正确的权限而无法删除 Oracle CDC 服务注册，则系统将会提示用户输入具有对 MSXDBCDC 数据库的更新权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 有关必须在“连接到 SQL Server”对话框中输入的数据的信息，请参阅 [Connection to SQL Server for Delete](../../integration-services/change-data-capture/connection-to-sql-server-for-delete.md)。  
  
### <a name="edit-the-cdc-service-properties"></a>编辑 CDC 服务属性  
 从 CDC 服务配置控制台右侧的 **“操作”** 窗格中，单击 **“属性”**。  
  
 也可以右键单击要编辑其属性的 CDC 服务，然后选择“属性”。  
  
## <a name="see-also"></a>另请参阅  
 [如何管理本地 CDC 服务](../../integration-services/change-data-capture/how-to-manage-a-local-cdc-service.md)  
  
  
