---
title: 将数据库迁移到 SQL Server on Linux |Microsoft 文档
description: 本文介绍在 Linux 上的迁移数据库和数据迁移到 SQL Server 的不同选项。
author: rothja
ms.author: jroth
manager: craigg
ms.date: 03/17/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.technology: linux
ms.assetid: 1619489d-377a-4f32-8930-d4f536539689
ms.custom: sql-linux
ms.openlocfilehash: c7b7225ae4e981244ee06194ea9e4ef595ba283d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2018
---
# <a name="migrate-databases-and-structured-data-to-sql-server-on-linux"></a>将数据库和结构化数据迁移到 Linux 上的 SQL Server 

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

你可以将你的数据库和数据迁移到运行在 Linux 上的 SQL Server 2017。 选用的方法取决于源数据和特定方案。 以下部分提供多种迁移方案的最佳做法。

## <a name="migrate-from-sql-server-on-windows"></a>从 Windows 上的 SQL Server 迁移
如果要将 Windows 上的 SQL Server 数据库迁移到 Linux 版 SQL Server 2017，建议的方法是使用 SQL Server 备份和还原。

1. 在 Windows 计算机上创建数据库的备份。
2. 将备份文件传输到目标 SQL Server Linux 计算机。
3. 在 Linux 计算机上还原备份。 

有关通过备份和还原迁移数据库的教程，请参阅下面的主题：

- [从 Windows 的 SQL Server 数据库还原到 Linux](sql-server-linux-migrate-restore-database.md)。

还可以将数据库导入 BACPAC 文件（包含数据库架构和数据的压缩文件）。 如果拥有 BACPAC 文件，可将此文件传输到 Linux 计算机，然后将其导入 SQL Server。 有关详细信息，请参阅以下主题：

- [导出和导入具有 SSMS 或 SqlPackage.exe 的数据库](sql-server-linux-migrate-ssms.md)

## <a name="migrate-from-other-database-servers"></a>从其他数据库服务器迁移
可将其他数据库系统上的数据库迁移到 Linux 版 SQL Server 2017。 这包括 Microsoft Access、 DB2、 MySQL、 Oracle 和 Sybase 数据库。 在此方案中，使用 SQL Server Management Assistant (SSMA) 自动执行到 Linux 上 SQL Server 的迁移。 有关详细信息，请参阅[使用 SSMA，若要将数据库迁移到 SQL Server on Linux](sql-server-linux-migrate-ssma.md)。  

## <a name="migrate-structured-data"></a>迁移结构化数据
还有用于导入原始数据的方法。 假设拥有从其他数据库或数据源导出的结构化数据文件。 在这种情况下，可以使用 bcp 工具批量插入数据。 或者，可以在 Windows 上运行 SQL Server Integration Services，将数据导入 Linux 上的 SQL Server 数据库。 使用 SQL Server Integration Services 可以在导入过程中对数据运行更复杂的转换。 

有关这些方法的详细信息，请参阅下列主题：

- [使用 bcp 大容量复制数据](sql-server-linux-migrate-bcp.md)
- [提取、 转换和加载数据使用 SSIS 的 Linux 上的 SQL Server](sql-server-linux-migrate-ssis.md) 
