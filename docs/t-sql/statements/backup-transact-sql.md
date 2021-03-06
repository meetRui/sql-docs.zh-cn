---
title: BACKUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- BACKUP_TSQL
- BACKUP
- BACKUP_DATABASE_TSQL
- BACKUP_LOG_TSQL
- BACKUP LOG
- BACKUP DATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], BACKUP statement
- backing up filegroups [SQL Server]
- backup file formats [SQL Server]
- backups [SQL Server]
- database backups [SQL Server], BACKUP statement
- multifamily media sets [SQL Server]
- mirrored media sets [SQL Server]
- backing up files [SQL Server]
- backup sets [SQL Server], BACKUP statement
- backup compression [SQL Server], BACKUP statement
- BACKUP statement
- backups [SQL Server], BACKUP statement
- backup history tables [SQL Server]
- transaction log backups [SQL Server], BACKUP LOG statement
- READ_WRITE_FILEGROUPS option
- BACKUP DATABASE statement
- concurrency [SQL Server], backups
- backing up databases [SQL Server]
- passwords [SQL Server], backups
- security [SQL Server], backups
- media families [SQL Server]
- BACKUP LOG statement
- backing up transaction logs [SQL Server]
- stripe sets [SQL Server]
- cross-platform backups
ms.assetid: 89a4658a-62f1-4289-8982-f072229720a1
caps.latest.revision: 275
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3c7d97d9c8ee56af89807f07cd335b16c50fbcc1
ms.sourcegitcommit: 02c889a1544b0859c8049827878d66b2301315f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2018
---
# <a name="backup-transact-sql"></a>BACKUP (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  备份完整的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库以创建数据库备份，或者备份数据库的一个或多个文件或文件组以创建文件备份 (BACKUP DATABASE)。 另外，在完整恢复模式或大容量日志恢复模式下备份数据库事务日志以创建日志备份 (BACKUP LOG)。 

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
``` 
Backing Up a Whole Database   
BACKUP DATABASE { database_name | @database_name_var }   
  TO <backup_device> [ ,...n ]   
  [ <MIRROR TO clause> ] [ next-mirror-to ]  
  [ WITH { DIFFERENTIAL -- Not supporterd in SQL Database Managed Instance
           | <general_WITH_options> [ ,...n ] } ]  
[;]  
  
Backing Up Specific Files or Filegroups  
BACKUP DATABASE { database_name | @database_name_var }   
 <file_or_filegroup> [ ,...n ]   
  TO <backup_device> [ ,...n ]   
  [ <MIRROR TO clause> ] [ next-mirror-to ]  
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]  
[;]  
  
Creating a Partial Backup  
BACKUP DATABASE { database_name | @database_name_var }   
 READ_WRITE_FILEGROUPS [ , <read_only_filegroup> [ ,...n ] ]  
  TO <backup_device> [ ,...n ]   
  [ <MIRROR TO clause> ] [ next-mirror-to ]  
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]  
[;]  
  
Backing Up the Transaction Log (full and bulk-logged recovery models)  
BACKUP LOG -- Not supported in SQL Database Managed Instance
  { database_name | @database_name_var }  
  TO <backup_device> [ ,...n ]   
  [ <MIRROR TO clause> ] [ next-mirror-to ]  
  [ WITH { <general_WITH_options> | \<log-specific_optionspec> } [ ,...n ] ]  
[;]  
  
<backup_device>::=   
 {  
   { logical_device_name | @logical_device_name_var }   
 | {   DISK -- Not supported in SQL Database Managed Instance
     | TAPE -- Not supported in SQL Database Managed Instance
     | URL } =   
     { 'physical_device_name' | @physical_device_name_var | 'NUL' }  
 }   
  
<MIRROR TO clause>::=  
 MIRROR TO <backup_device> [ ,...n ]  
  
<file_or_filegroup>::=  
 {  
   FILE = { logical_file_name | @logical_file_name_var }   
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }  
 }   
  
<read_only_filegroup>::=  
FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }  
  
<general_WITH_options> [ ,...n ]::=   
--Backup Set Options  
   COPY_ONLY -- Only backup set option supported by SQL Database Managed Instance  
 | { COMPRESSION | NO_COMPRESSION }   
 | DESCRIPTION = { 'text' | @text_variable }   
 | NAME = { backup_set_name | @backup_set_name_var }   
 | CREDENTIAL  
 | ENCRYPTION  
 | FILE_SNAPSHOT  --Not supported in SQL Database Managed Instance
 | { EXPIREDATE = { 'date' | @date_var }   
        | RETAINDAYS = { days | @days_var } }   
  
--Media Set Options  
   { NOINIT | INIT }   
 | { NOSKIP | SKIP }   
 | { NOFORMAT | FORMAT }   
 | MEDIADESCRIPTION = { 'text' | @text_variable }   
 | MEDIANAME = { media_name | @media_name_variable }   
 | BLOCKSIZE = { blocksize | @blocksize_variable }   
  
--Data Transfer Options  
   BUFFERCOUNT = { buffercount | @buffercount_variable }   
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }  
  
--Error Management Options  
   { NO_CHECKSUM | CHECKSUM }  
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Compatibility Options  
   RESTART   
  
--Monitoring Options  
   STATS [ = percentage ]   
  
--Tape Options. These are not supported in SQL Database Managed Instance
   { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }   
  
--Log-specific Options. These are not supported in SQL Database Managed Instance 
   { NORECOVERY | STANDBY = undo_file_name }  
 | NO_TRUNCATE  
  
--Encryption Options  
 ENCRYPTION (ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY } , encryptor_options ) <encryptor_options> ::=   
   SERVER CERTIFICATE = Encryptor_Name | SERVER ASYMMETRIC KEY = Encryptor_Name   
```  
  
## <a name="arguments"></a>参数  

DATABASE  
 指定一个完整数据库备份。 如果指定了一个文件和文件组的列表，则仅备份该列表中的文件和文件组。 在进行完整数据库备份或差异数据库备份的过程中，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会备份足够多的事务日志，以便在还原备份时生成一个一致的数据库。  
  
 还原由 BACKUP DATABASE（“数据备份”）创建的备份时，将还原整个备份。 只有日志备份才能还原到备份中的特定时间或事务。  
  
> [!NOTE]  
> 对 master 数据库，只能执行完整数据库备份。  
  
LOG 适用范围：SQL Server

 指定仅备份事务日志。 该日志是从上一次成功执行的日志备份到当前日志的末尾。 必须创建完整备份，才能创建第一个日志备份。  
  
 通过在 [RESTORE LOG](../../t-sql/statements/restore-statements-transact-sql.md) 语句中指定 `WITH STOPAT`、`STOPATMARK` 或 `STOPBEFOREMARK`，可以将日志备份还原到备份中的特定时间或事务。  
  
> [!NOTE]  
>  执行典型日志备份后，如果没有指定 `WITH NO_TRUNCATE` 或 `COPY_ONLY`，某些事务日志记录将变为不活动状态。 一个或多个虚拟日志文件中的所有记录变为不活动状态后，日志将被截断。 如果日志在常规日志备份后未被截断，则可能是某些操作延迟了日志截断。 有关详细信息，请参阅[可能延迟日志截断的因素](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation)。  
  
 { database_name | @***database_name_var } 是备份事务日志、部分数据库或完整的数据库时所用的源数据库 *。如果作为变量 (**@***database_name_var) 提供，则可以将此名称指定为字符串常量 (*@database_name_var=***database name) 或指定为字符串数据类型（ntext 或 text 数据类型除外）的变量***。  
  
> [!NOTE]  
> 不能备份数据库镜像伙伴关系中的镜像数据库。  
  
\<file_or_filegroup> [ **,**...*n* ]  
 只能与 BACKUP DATABASE 一起使用，用于指定某个数据库文件或文件组包含在文件备份中，或指定某个只读文件或文件组包含在部分备份中。  
  
 FILE **=** { *logical_file_name* | **@***logical_file_name_var* }  
 文件或变量的逻辑名称，其值等于要包含在备份中的文件的逻辑名称。  
  
 FILEGROUP **=** { *logical_filegroup_name* | **@***logical_filegroup_name_var* }  
 文件组或变量的逻辑名称，其值等于要包含在备份中的文件组的逻辑名称。 在简单恢复模式下，只允许对只读文件组执行文件组备份。  
  
> [!NOTE]  
> 如果数据库的大小和性能要求使得进行数据库备份不切实际，则应考虑使用文件备份。 NUL 设备可用于测试备份的性能，但不应在生产环境中使用。
  
 *n*  
 一个占位符，表示可以在逗号分隔的列表中指定多个文件和文件组。 数量不受限制。 
  
 有关详细信息，请参阅[完整文件备份 (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md) 和[备份文件和文件组 (SQL Server)](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)。  
  
 READ_WRITE_FILEGROUPS [ **,** FILEGROUP = { *logical_filegroup_name* | **@***logical_filegroup_name_var* } [ **,**...* n* ] ]  
 指定部分备份。 部分备份包括数据库中的所有读/写文件：主文件组和任何读/写辅助文件组，以及任何指定的只读文件或文件组。  
  
 READ_WRITE_FILEGROUPS  
 指定在部分备份中备份所有读/写文件组。 如果数据库是只读的，则 READ_WRITE_FILEGROUPS 仅包括主文件组。  
  
> [!IMPORTANT]  
> 使用 FILEGROUP 而不是 READ_WRITE_FILEGROUPS 显式列出读/写文件组将会创建文件备份。  
  
 FILEGROUP = { *logical_filegroup_name* | **@***logical_filegroup_name_var* }  
只读文件组或变量的逻辑名称，其值等于要包含在部分备份中的只读文件组的逻辑名称。 有关详细信息，请参阅本主题前面的“\<file_or_filegroup>”。
  
 *n*  
 一个占位符，表示可以在逗号分隔的列表中指定多个只读文件组。  
  
 有关部分备份的详细信息，请参阅[部分备份 (SQL Server)](../../relational-databases/backup-restore/partial-backups-sql-server.md)。  
  
TO \<backup_device> [ ,...n ] 指示附带的[备份设备](../../relational-databases/backup-restore/backup-devices-sql-server.md)集是一个未镜像的媒体集，或者是镜像媒体集中的第一批镜像（为其声明了一个或多个 MIRROR TO 子句）。  
  
\<backup_device> 适用范围：SQL Server。指定用于备份操作的逻辑备份设备或物理备份设备。  
  
 { logical_device_name | @logical_device_name_var } 适用范围：SQL Server。要将数据库备份到的备份设备的逻辑名称 *。逻辑名称必须遵守标识符规则。如果作为变量 (@logical_device_name_var) 提供，则可以将该备份设备名称指定为字符串常量 (@logical_device_name_var*= logical backup device name) 或任何字符串数据类型（ntext 或 text 数据类型除外）的变量*。  
  
 { DISK | TAPE | URL} = { 'physical_device_name' | *@***physical_device_name_var | 'NUL' } 适用范围：磁盘、磁带和用于 SQL Server 的 URL****。 仅 URL 适用于 SQL 数据库托管实例。指定磁盘文件或磁带设备，或者 Windows Azure 存储服务。 此 URL 格式用于创建到 Windows Azure 存储服务的备份。 有关详细信息和示例，请参阅[使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)。 有关教程的信息，请参阅[教程：SQL Server 备份和还原到 Windows Azure Blob 存储服务](~/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)。 

> [!NOTE] 
> NUL 磁盘设备将弃用发送给它的所有信息，且仅应用于测试。 这不适用于生产用途。
  
> [!IMPORTANT]  
> 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 到 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]，备份到 URL 时只能备份到单个设备。 备份到 URL 时，若要备份到多个设备，必须使用 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 以及共享访问签名 (SAS) 令牌。 有关创建共享访问签名的示例，请参阅 [SQL Server 备份到 URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md) 和[使用 Powershell 简化在 Azure 存储空间中使用共享访问签名 (SAS) 令牌创建 SQL 凭据的过程](http://blogs.msdn.com/b/sqlcat/archive/2015/03/21/simplifying-creation-sql-credentials-with-shared-access-signature-sas-keys-on-azure-storage-containers-with-powershell.aspx)。  
  
URL 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]）和 SQL 数据库托管实例。  
  
 如果某一磁盘设备不存在，也可以在 BACKUP 语句中指定它。 如果存在物理设备且 BACKUP 语句中未指定 INIT 选项，则备份将追加到该设备。  
 
> [!NOTE] 
> NUL 设备将弃用发送到此文件的所有输入，但备份仍将所有页面标记为备份。
  
 有关详细信息，请参阅 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。  
  
> [!NOTE]  
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中将删除 TAPE 选项。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。  
  
 *n*  
 一个占位符，表示最多可以在逗号分隔的列表中指定 64 个备份设备。  
  
MIRROR TO \<backup_device> [ ,...n ] 指定一组辅助备份设备（最多三个），其中每个设备都镜像 TO 子句中指定的备份设备。 MIRROR TO 子句和 TO 子句必须指定相同类型和数量的备份设备。 最多可以使用三个 MIRROR TO 子句。  
  
 此选项仅在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Enterprise 版中可用。  
  
> [!NOTE]  
> 对于 MIRROR TO = DISK，BACKUP 自动决定磁盘设备合适的块大小。 有关块大小的详细信息，请参阅此表后面的 "BLOCKSIZE"。  
  
有关 \<backup_device> 的信息，请参阅本部分前面的“\<backup_device>”。
  
 *n*  
 一个占位符，表示最多可以在逗号分隔的列表中指定 64 个备份设备。 MIRROR TO 子句中的设备数必须等于 TO 子句中的设备数。  
  
 有关详细信息，请参阅本主题后面[备注](#general-remarks)部分中的“镜像媒体集中的媒体簇”。  
  
 [ *next-mirror-to* ]  
 一个占位符，表示一个 BACKUP 语句除了包含一个 TO 子句外，最多还可包含三个 MIRROR TO 子句。  
  
### <a name="with-options"></a>WITH 选项  
 指定要用于备份操作的选项。  
  
 CREDENTIAL  
**适用范围**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]）和 SQL 数据库托管实例。  
 仅在创建到 Windows Azure Blob 存储服务的备份时使用。  
  
 FILE_SNAPSHOT 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]）。

 用于在使用 Azure Blob 存储服务存储所有 SQL Server 数据库文件时，创建数据库文件的 Azure 快照。 有关详细信息，请参阅 [Microsoft Azure 中的 SQL Server 数据文件](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 快照备份将数据库文件（数据和日志文件）的 Azure 快照保持为一致的状态。 一组一致的 Azure 快照构成备份并记录在备份文件中。 `BACKUP DATABASE TO URL WITH FILE_SNAPSHOT` 和 `BACKUP LOG TO URL WITH FILE_SNAPSHOT` 之间的唯一区别是后者会截断事务日志，而前者不会。 借助 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 快照备份，在完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 建立备份链时所需的初始完整备份之后，只需单个事务日志备份即可将数据库还原到事务日志备份的时间点。 此外，只需两次事务日志备份即可将数据库还原到两次事务日志备份之间的时间点。  
    
 DIFFERENTIAL  
**适用范围：** SQL Server。只能与 BACKUP DATABASE 一起使用，指定数据库备份或文件备份应该只包含上次完整备份后更改的数据库或文件部分。 差异备份一般会比完整备份占用更少的空间。 对于上一次完整备份后执行的所有单个日志备份，使用该选项可以不必再进行备份。  
  
> [!NOTE]  
> 默认情况下，`BACKUP DATABASE` 创建完整备份。  
  
 有关详细信息，请参阅 [差异备份 (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md)。  
  
 ENCRYPTION  
 用于指定将备份加密。 可指定加密备份所用的加密算法，或指定 `NO_ENCRYPTION` 以不加密备份。 建议进行加密以帮助保护备份文件的安全。 可指定的算法的列表如下：  
  
-   `AES_128`  
-   `AES_192`  
-   `AES_256`  
-   `TRIPLE_DES_3KEY`  
-   `NO_ENCRYPTION`    

如果决定加密，则还必须使用加密程序选项指定加密程序：  
  
-   SERVER CERTIFICATE = Encryptor_Name  
-   SERVER ASYMMETRIC KEY = Encryptor_Name  
  
> [!WARNING]  
> 将加密与 `FILE_SNAPSHOT` 参数一起使用时，使用指定的加密算法对元数据文件本身进行加密，并且系统会验证是否已完成对数据库的[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)。 不会再对数据本身进行其他加密。 如果未加密数据库或发出备份语句之前未完成加密，则备份失败。  
  
**备份集选项**  
  
这些选项对此备份操作创建的备份集进行操作。  
  
> [!NOTE]  
> 若要为还原操作指定备份集，请使用 `FILE = <backup_set_file_number>` 选项。 有关如何指定备份集的详细信息，请参阅 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md) 中的“指定备份集”。
  
 COPY_ONLY 适用范围：SQL Server 和 SQL 数据库托管实例。指定备份为“仅复制备份”，该备份不影响正常的备份顺序。 仅复制备份是独立于定期计划的常规备份而创建的。 仅复制备份不会影响数据库的总体备份和还原过程。  
  
 应在出于特殊目的而进行备份的情况下使用仅复制备份，例如在进行联机文件还原前备份日志。 通常，仅复制日志备份仅使用一次即被删除。  
  
-   与 `BACKUP DATABASE` 一起使用时，`COPY_ONLY` 选项创建的完整备份不能用作差异基准。 差异位图不会被更新，因此差异备份的表现就像仅复制备份不存在一样。 后续差异备份将最新的常规完整备份用作它们的基准。  
  
    > [!IMPORTANT]  
    > 如果将 `DIFFERENTIAL` 和 `COPY_ONLY` 一起使用，则忽略 `COPY_ONLY` 并创建差异备份。  
  
-   与 `BACKUP LOG` 一起使用时，`COPY_ONLY` 选项将创建“仅复制日志备份”，该备份不会截断事务日志。 仅复制日志备份对日志链没有任何影响，因此其他日志备份的表现就像仅复制备份不存在一样。  
  
有关详细信息，请参阅[仅复制备份 (SQL Server)](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)。  
  
{ COMPRESSION | NO_COMPRESSION }  
仅适用于 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] 和更高版本；指定是否对此备份执行[备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)，覆盖服务器级默认设置。  
  
安装时，默认行为是不进行备份压缩。 但此默认设置可通过设置 [backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) 服务器配置选项进行更改。 有关查看此选项的当前值的信息，请参阅[查看或更改服务器属性面板 (SQL Server)](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)。  

有关对支持[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) 的数据库使用备份压缩的信息，请参阅[备注](#general-remarks)部分。
  
COMPRESSION  
显式启用备份压缩。  
  
NO_COMPRESSION  
显式禁用备份压缩。  
  
DESCRIPTION = { 'text' | @text_variable }****  
指定说明备份集的自由格式文本。 该字符串最长可达 255 个字符。  
  
NAME **=** { *backup_set_name* | **@***backup_set_var* }  
指定备份集的名称。 名称最长可达 128 个字符。 如果未指定 NAME，它将为空。  
  
{ EXPIREDATE **='***date***'** | RETAINDAYS **=** *天数* }  
指定允许覆盖该备份的备份集的日期。 如果同时使用这两个选项，RETAINDAYS 的优先级别将高于 EXPIREDATE。  
  
如果这两个选项均未指定，则过期日期由 mediaretention 配置设置确定。 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。   
  
> [!IMPORTANT]  
> 这些选项仅仅阻止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 覆盖文件。 用其他方法仍可擦除磁带，而通过操作系统也可以删除磁盘文件。 有关过期验证的详细信息，请参阅本主题中的 SKIP 和 FORMAT。  
  
EXPIREDATE **=** { **'***date***'** | **@***date_var* } 指定备份何时到期并可覆盖。如果作为变量 (@* date_var*) 提供，则该日期必须采用已配置系统日期/时间的格式，并指定为下列类型之一：  
  
-   字符串常量 (@*date_var* **=** date)  
-   字符串数据类型（ntext 或 text 数据类型除外）的变量  
-   smalldatetime  
-   datetime 变量  
  
例如：  
  
-   `'Dec 31, 2020 11:59 PM'`  
-   `'1/1/2021'`  
  
有关如何指定 datetime 值的信息，请参阅[日期和时间类型](../../t-sql/data-types/date-and-time-types.md)。  
  
> [!NOTE]  
> 若要忽略过期日期，请使用 `SKIP` 选项。  
  
RETAINDAYS **=** { *days* | **@***days_var* } 指定必须经过多少天才可以覆盖该备份媒体集。如果作为变量 (**@***days_var*) 提供，则必须指定为整数。  
  
**媒体集选项**  
  
这些选项作为一个整体对介质集进行操作。  
  
{ **NOINIT** | INIT }  
 控制备份操作是追加到还是覆盖备份介质中的现有备份集。 默认为追加到介质中最新的备份集 (NOINIT)。  
  
> [!NOTE]  
> 有关 { **NOINIT** | INIT } and { **NOSKIP** | SKIP } 之间交互的信息，请参阅本主题后面的[备注](#general-remarks)。  
  
NOINIT  
 表示备份集将追加到指定的介质集上，以保留现有的备份集。 如果为介质集定义了介质密码，则必须提供密码。 NOINIT 是默认设置。  
  
有关详细信息，请参阅 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)。  
  
INIT  
 指定应覆盖所有备份集，但是保留介质标头。 如果指定了 INIT，将覆盖该设备上所有现有的备份集（如果条件允许）。 默认情况下，BACKUP 将检查下列条件，如果其中的任一条件存在，都不会覆盖备份介质：  
  
-   所有备份集都未过期。 有关详细信息，请参阅 `EXPIREDATE` 和 `RETAINDAYS` 选项。  
-   如果 BACKUP 语句给出了备份集名，则该备份集名与备份介质上的名称不匹配。 有关详细信息，请参阅本部分前面介绍的 NAME 选项。  
  
若要替代这些检查，请使用 `SKIP` 选项。  
  
有关详细信息，请参阅 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)。  
  
{ **NOSKIP** | SKIP }  
控制备份操作是否在覆盖介质中的备份集之前检查它们的过期日期和时间。  
  
> [!NOTE]  
> 有关 { **NOINIT** | INIT } and { **NOSKIP** | SKIP } 之间交互的信息，请参阅本主题后面的“备注”。  
  
NOSKIP  
指示 BACKUP 语句在可以覆盖介质上的所有备份集之前先检查它们的过期日期。 这是默认行为。  
  
SKIP  
禁用备份集的过期和名称检查，这些检查一般由 BACKUP 语句执行以防覆盖备份集。 有关 { NOINIT | INIT } 和 { NOSKIP | SKIP } 之间交互的信息，请参阅本主题后面的“备注”。  
若要查看备份集的过期日期，请查询 [backupset](../../relational-databases/system-tables/backupset-transact-sql.md) 历史记录表的 **expiration_date** 列。  
  
{ **NOFORMAT** | FORMAT }  
指定是否应该在用于此备份操作的卷上写入介质标头，以覆盖任何现有的介质标头和备份集。  
  
NOFORMAT  
指定备份操作在用于此备份操作的介质卷上保留现的有介质标头和备份集。 这是默认行为。  
  
FORMAT  
指定创建新的介质集。 FORMAT 将使备份操作在用于备份操作的所有介质卷上写入新的介质标头。 卷的现有内容将变为无效，因为覆盖了任何现有的介质标头和备份集。  
  
> [!IMPORTANT]  
> 请谨慎使用 `FORMAT`。 格式化介质集的任何一个卷都将使整个介质集不可用。 例如，如果初始化现有条带介质集中的单个磁带，则整个介质集都将变得不可用。  
  
指定 FORMAT 即表示 `SKIP`；`SKIP` 无需显式声明。  
  
MEDIADESCRIPTION **=** { *text* | **@***text_variable* }  
指定介质集的自由格式文本说明，最多为 255 个字符。  
  
MEDIANAME **=** { *media_name* | **@***media_name_variable* }  
指定整个备份介质集的介质名称。 介质名称的长度不能多于 128 个字符，如果指定了 `MEDIANAME`，则该名称必须匹配备份卷上已存在的先前指定的介质名称。 如果未指定该选项或指定了 SKIP 选项，将不会对介质名称进行验证检查。  
  
BLOCKSIZE **=** { *blocksize* | **@***blocksize_variable* }  
用字节数来指定物理块的大小。 支持的大小是 512、1024、2048、4096、8192、16384、32768 和 65536 (64 KB) 字节。 对于磁带设备默认为 65536，其他情况为 512。 通常，由于 BACKUP 自动选择适合于设备的块大小，因此不需要此选项。 显式声明块大小将覆盖自动选择块大小。  
  
如果要建立一个计划在 CD-ROM 上进行复制和还原的备份，请指定 BLOCKSIZE=2048。  
  
> [!NOTE]  
> 通常，只有写入磁带设备时，此选项才会影响性能。  
  
**数据传输选项**  
  
BUFFERCOUNT **=** { *buffercount* | **@***buffercount_variable* }  
指定用于备份操作的 I/O 缓冲区总数。 可以指定任何正整数；但是，较大的缓冲区数可能导致由于 Sqlservr.exe 进程中的虚拟地址空间不足而发生“内存不足”错误。  
  
缓冲区使用的总计空间由以下内容确定：buffercount/maxtransfersize。  
  
> [!NOTE]  
> 有关使用 `BUFFERCOUNT` 选项的重要信息，请参阅[不正确的 BufferCount 数据传输选项可导致 OOM 情况](http://blogs.msdn.com/b/sqlserverfaq/archive/2010/05/06/incorrect-buffercount-data-transfer-option-can-lead-to-oom-condition.aspx)博客。  
  
MAXTRANSFERSIZE **=** { *maxtransfersize* | ***@** maxtransfersize_variable* } 指定要在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和备份介质之间使用的最大传输单元（字节）。 可能的值是 65536 字节 (64 KB) 的倍数，最多可到 4194304 字节 (4 MB)。  

> [!NOTE]  
> 使用 SQL 编写器服务创建备份时，如果数据库已配置 [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md)（文件流）或包含[内存优化文件组](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)，则恢复时的 `MAXTRANSFERSIZE` 应大于或等于创建备份时使用的 `MAXTRANSFERSIZE`。 

> [!NOTE]  
> 对于具有单个数据文件且支持[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) 的数据库，默认 `MAXTRANSFERSIZE` 为 65536 (64 KB)。 对于非 TDE 加密数据库，使用备份到 DISK 时，默认 `MAXTRANSFERSIZE` 为 1048576 (1 MB)，使用 VDI 或 TAPE.时为 65536 (64 KB)。
> 有关对 TDE 加密数据库使用备份压缩的详细信息，请参阅[备注](#general-remarks)部分。
  
**错误管理选项**  
  
使用这些选项可以确定是否为备份操作启用了备份校验和，以及备份操作是否在遇到错误时停止。  
  
{ **NO_CHECKSUM** | CHECKSUM }  
 控制是否启用备份校验和。  
  
NO_CHECKSUM  
显式禁用备份校验和的生成（以及页校验和的验证）。 这是默认行为。  
  
CHECKSUM  
如果此选项已启用并且可用，则指定备份操作将验证每页的校验和及页残缺，并生成整个备份的校验和。  
  
使用备份校验和可能会影响工作负荷以及备份吞吐量。  
  
有关详细信息，请参阅[在备份和还原期间可能的媒体错误 (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)。  
  
{ **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR }  
控制备份操作在遇到页校验和错误后是停止还是继续。  
  
STOP_ON_ERROR  
如果未验证页校验和，则指示 BACKUP 失败。 这是默认行为。  
  
CONTINUE_AFTER_ERROR  
指示 BACKUP 继续执行，不管是否遇到无效校验和或页撕裂之类的错误。  
  
数据库损坏时，如果无法使用 NO_TRUNCATE 选项备份日志尾部，则可以通过指定 CONTINUE_AFTER_ERROR 而不是 NO_TRUNCATE 尝试执行[尾日志备份](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)。  
  
有关详细信息，请参阅[在备份和还原期间可能的媒体错误 (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)。  
  
**兼容性选项**  
  
RESTART  
从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始不起作用。 此版本接受该选项，以便与旧版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 保持兼容。  
  
**监视选项**  
  
STATS [ **=** *percentage* ]  
 每当另一个百分比完成时显示一条消息，并用于测量进度。 如果省略百分比，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在每完成 10% 就显示一条消息。  
  
STATS 选项报告截止报告下一个间隔的阈值时的完成百分比。 这是指定百分比的近似值；例如，当 STATS=10 时，如果完成进度为 40%，则该选项可能显示 43%。 对于较大的备份集，这不是问题，因为完成百分比在已完成的 I/O 调用之间变化非常缓慢。  
  
**磁带选项**  
**适用范围**：SQL Server  
这些选项只用于 TAPE 设备。 如果使用的是非磁带设备，则会忽略这些选项。  
  
{ **REWIND** | NOREWIND }  
REWIND 适用范围：SQL Server。指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 释放和倒带磁带。 REWIND 是默认设置。  
  
NOREWIND 适用范围：SQL Server。指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在备份操作之后让磁带一直处于打开状态。 在对磁带执行多个备份操作时，可以使用此选项来帮助改进性能。  
  
NOREWIND 包含 NOUNLOAD，并且这些选项在单个 BACKUP 语句中不兼容。  
  
> [!NOTE]  
> 如果使用 `NOREWIND`，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将一直保留磁带机的所有权，直到在同一进程中运行的 BACKUP 或 RESTORE 语句使用 `REWIND` 或 `UNLOAD` 选项或服务器实例关闭为止。 磁带保持打开将防止其他进程访问磁带。 有关如何显示打开的磁带列表和如何将打开的磁带关闭的信息，请参阅[备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。  
  
{ **UNLOAD** | NOUNLOAD }    
**适用范围**：SQL Server 
> [!NOTE]  
> `UNLOAD` 和 `NOUNLOAD` 会话设置可在整个会话期间存在，或者在通过指定其他设置而进行重置之前一直存在。  
  
UNLOAD 适用范围：SQL Server   
 指定在备份完成后自动重绕并卸载磁带。 会话开始时 UNLOAD 是默认值。 
  
NOUNLOAD 适用范围：SQL Server，指定在 BACKUP 操作之后磁带继续在磁带机中加载。  
  
> [!NOTE]  
> 备份到磁带备份设备时，`BLOCKSIZE` 选项会影响备份操作的性能。 通常，只有写入磁带设备时，此选项才会影响性能。  
  
**特定于日志的选项**  
**适用范围**：SQL Server  
这些选项仅与 `BACKUP LOG` 一起使用。  
  
> [!NOTE]  
> 如果不想进行日志备份，则请使用简单恢复模式。 有关详细信息，请参阅[恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)。  
  
{ NORECOVERY | STANDBY **=** *undo_file_name* }  
  NORECOVERY 适用范围：SQL Server   
  备份日志的尾部并使数据库处于 RESTORING 状态。 当将故障转移到辅助数据库或在执行 RESTORE 操作前保存日志尾部时，NORECOVERY 很有用。  
  
  若要执行最大程度的日志备份（跳过日志截断）并自动将数据库置于 RESTORING 状态，请同时使用 `NO_TRUNCATE` 和 `NORECOVERY` 选项。  
  
  STANDBY = standby_file_name  
适用范围：SQL Server**********   
  备份日志的尾部并使数据库处于只读和 STANDBY 状态。 将 STANDBY 子句写入备用数据（执行回滚，但需带进一步还原选项）。 使用 STANDBY 选项等同于 BACKUP LOG WITH NORECOVERY 后跟 RESTORE WITH STANDBY。  
  
  使用备用模式需要一个备用文件，该文件由 standby_file_name 指定，其位置存储于数据库的日志中。 如果指定的文件已经存在，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]会覆盖该文件；如果指定的文件不存在，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]将创建它。 备用文件将成为数据库的一部分。  
  
  该文件将保存对回滚所做的更改，如果要在以后应用 RESTORE LOG 操作，则必须反转这些更改。 必须有足够的磁盘空间供备用文件增长，以使备用文件能够包含数据库中由回滚的未提交事务修改的所有不重复的页。  
  
NO_TRUNCATE  
**适用范围**：SQL Server  
指定不截断日志，并使 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 尝试执行备份，而不考虑数据库的状态。 因此，使用 `NO_TRUNCATE` 执行的备份可能具有不完整的元数据。 该选项允许在数据库损坏时备份日志。  
  
BACKUP LOG 的 NO_TRUNCATE 选项相当于同时指定 COPY_ONLY 和 CONTINUE_AFTER_ERROR。  
  
如果不使用 `NO_TRUNCATE` 选项，则数据库必须处于 ONLINE 状态。 如果数据库处于 SUSPENDED 状态，则可以通过指定 `NO_TRUNCATE` 来创建备份。 但是，如果数据库处于 OFFLINE 或 EMERGENCY 状态，即便使用了 `NO_TRUNCATE`，也不允许执行 BACKUP。 有关数据库状态的详细信息，请参阅[数据库状态](../../relational-databases/databases/database-states.md)。  
  
## <a name="about-working-with-sql-server-backups"></a>关于使用 SQL Server 备份  
 本节简要说明了以下基本备份概念：  
  
 [备份类型](#Backup_Types)  
 [事务日志截断](#Tlog_Truncation)  
 [格式化备份介质](#Formatting_Media)  
 [使用备份设备和媒体集](#Backup_Devices_and_Media_Sets)  
 [还原 SQL Server 备份](#Restoring_Backups)  
  
> [!NOTE]  
> 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中备份的说明，请参阅[备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)。  
  
###  <a name="Backup_Types"></a> 备份类型  
 支持的备份类型取决于数据库的恢复模式，如下所示：  
  
-   所有恢复模式都支持数据的完整备份和差异备份。  
  
    |备份范围|备份类型|  
    |---------------------|------------------|  
    |整个数据库|[数据库备份](../../relational-databases/backup-restore/full-database-backups-sql-server.md)涵盖整个数据库。<br /><br /> 或者，每个数据库备份都可以充当由一个或多个[差异数据库备份](../../relational-databases/backup-restore/differential-backups-sql-server.md)构成的系列的基础。|  
    |部分数据库|[部分备份](../../relational-databases/backup-restore/partial-backups-sql-server.md)涵盖读/写文件组，也可能涵盖一个或多个只读文件或文件组。<br /><br /> 或者，每个部分备份都可以充当由一个或多个[差异部分备份](../../relational-databases/backup-restore/differential-backups-sql-server.md)构成的系列的基础。|  
    |文件或文件组|[文件备份](../../relational-databases/backup-restore/full-file-backups-sql-server.md)涵盖一个或多个文件或文件组，仅与包含多个文件组的数据库相关。 在简单恢复模式下，文件备份实质上仅限于只读辅助文件组。<br /> 或者，每个文件备份都可以充当由一个或多个[差异文件备份](../../relational-databases/backup-restore/differential-backups-sql-server.md)构成的系列的基础。|  
  
-   在完整恢复模式或大容量日志恢复模式下，常规备份还包括顺序“事务日志备份”（或称“日志备份”），这是必需的备份。 每个日志备份均涵盖创建备份时处于活动状态的事务日志部分，并包括在上次日志备份中没有备份的所有日志记录。  
  
     若要以增加管理开销为代价最大限度地降低工作丢失的风险，您应该安排对日志进行频繁的备份。 在完整备份之间安排差异备份可减少数据还原后需要还原的日志备份数，从而缩短还原时间。  
  
     建议您将日志备份和数据库备份分别放在不同的卷上。  
  
    > [!NOTE]  
    > 必须创建完整备份，才能创建第一个日志备份。  
  
-   “仅复制备份”是特殊用途的完整备份或日志备份，它独立于正常的常规备份顺序。 若要创建仅复制备份，请在 BACKUP 语句中指定 COPY_ONLY 选项。 有关详细信息，请参阅[仅复制备份 (SQL Server)](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)。  
  
###  <a name="Tlog_Truncation"></a> 事务日志截断  
 若要避免填满数据库的事务日志，例行备份至关重要。 在简单恢复模式下，备份了数据库后会自动截断日志，而在完整恢复模式下，只有备份了事务日志后方才截断日志。 但是，截断过程有时也可能发生延迟。 有关延迟日志截断的因素的信息，请参阅[事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)。  
  
> [!NOTE]  
> 已停用 `BACKUP LOG WITH NO_LOG` 和 `WITH TRUNCATE_ONLY` 选项。 使用完整恢复模式或大容量日志恢复模式时，如果必须删除数据库中的日志备份链，请切换至简单恢复模式。 有关详细信息，请参阅[查看或更改数据库的恢复模式 (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)。  
  
###  <a name="Formatting_Media"></a> 格式化备份介质  
 当且仅当满足任何以下条件时，BACKUP 语句才会格式化备份介质：  
  
-   未指定 `FORMAT` 选项。  
-   介质为空。  
-   操作正在写入延续磁带。  
  
###  <a name="Backup_Devices_and_Media_Sets"></a> 使用备份设备和媒体集  
  
#### <a name="backup-devices-in-a-striped-media-set-a-stripe-set"></a>条带媒体集（条带集）中的备份设备  
 “条带集”是一组磁盘文件，其中的数据划分为若干块并按固定顺序分发。 条带集中使用的备份设备数目必须保持不变（除非以 `FORMAT` 命令重新初始化介质）。  
  
 下面的示例将 [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] 数据库的备份写入使用三个磁盘文件的新条带介质集。  
  
```sql  
BACKUP DATABASE AdventureWorks2012  
TO DISK='X:\SQLServerBackups\AdventureWorks1.bak',   
DISK='Y:\SQLServerBackups\AdventureWorks2.bak',   
DISK='Z:\SQLServerBackups\AdventureWorks3.bak'  
WITH FORMAT,  
   MEDIANAME = 'AdventureWorksStripedSet0',  
   MEDIADESCRIPTION = 'Striped media set for AdventureWorks2012 database;  
GO  
```  
  
 在备份设备已定义为条带集的组成部分后，将不能用于单个设备备份，除非指定了 FORMAT。 同样，一个含有非条带备份的备份设备不能用于条带集，除非指定了 FORMAT。 若要拆分条带备份集，请使用 FORMAT。  
  
 如果写入介质标头时未指定 MEDIANAME 或 MEDIADESCRIPTION，则与空项对应的介质标头字段将为空。  
  
#### <a name="working-with-a-mirrored-media-set"></a>使用镜像媒体集  
 通常，备份是非镜像的，而且 BACKUP 语句仅包含一个 TO 子句。 但是，每个介质集可能总共包含四个镜像。 对于镜像介质集，备份操作写入到多组备份设备。 每组备份设备均包含镜像介质集中的一个镜像。 每个镜像都必须使用相同数量和类型的物理备份设备，而且这些设备必须都具有相同的属性。  
  
 若要备份到镜像介质集，则所有的镜像服务器必须存在。 若要备份到镜像媒体集，请指定 `TO` 子句来指定第一个镜像，并为其他每个镜像指定 `MIRROR TO` 子句。  
  
 对于镜像媒体集，每个 `MIRROR TO` 子句列出的设备数量和类型都必须与 TO 子句列出的相同。 下面的示例写入到包含两个镜像并在每个镜像中使用三个设备的镜像介质集：  
  
```sql  
BACKUP DATABASE AdventureWorks2012  
TO DISK='X:\SQLServerBackups\AdventureWorks1a.bak',   
  DISK='Y:\SQLServerBackups\AdventureWorks2a.bak',   
  DISK='Z:\SQLServerBackups\AdventureWorks3a.bak'  
MIRROR TO DISK='X:\SQLServerBackups\AdventureWorks1b.bak',   
  DISK='Y:\SQLServerBackups\AdventureWorks2b.bak',   
  DISK='Z:\SQLServerBackups\AdventureWorks3b.bak';  
GO  
```  
  
> [!IMPORTANT]  
> 此示例旨在允许您在本地系统上对其进行测试。 实际上，备份到同一驱动器中的多个设备会降低性能并不存在冗余，而镜像介质集正是为冗余而设计的。  
  
##### <a name="media-families-in-mirrored-media-sets"></a>镜像媒体集中的媒体簇  
 BACKUP 语句的 `TO` 子句中指定的每个备份设备均对应于一个媒体簇。 例如，如果 `TO` 子句列出三个设备，则 BACKUP 将数据写入三个媒体簇。 在镜像介质集中，每个镜像都必须包含各个介质簇的副本。 这正是各个镜像中的设备数量必须相同的原因。  
  
 当对每个镜像列出多个设备时，这些设备的顺序将决定将哪个介质簇写入特定的设备。 例如，在每个设备列表中，第二个设备都对应于第二个介质簇。 对于上例中的设备，设备与介质簇间的对应关系如下表所示。  
  
|镜像|媒体簇 1|媒体簇 2|介质簇 3|  
|---------|---------|---------|---------|  
|0|`Z:\AdventureWorks1a.bak`|`Z:\AdventureWorks2a.bak`|`Z:\AdventureWorks3a.bak`|  
|@shouldalert|`Z:\AdventureWorks1b.bak`|`Z:\AdventureWorks2b.bak`|`Z:\AdventureWorks3b.bak`|  
  
 介质簇必须总是备份到特定镜像中的同一个设备。 因此，每次使用现有介质集时，请按照创建介质集时指定的相同顺序列出各个镜像的设备。  
  
 有关镜像媒体集的详细信息，请参阅[镜像备份媒体集 (SQL Server)](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)。 有关媒体集和媒体簇的常规信息，请参阅[媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)。  
  
###  <a name="Restoring_Backups"></a> 还原 SQL Server 备份  
 若要还原数据库，选择联机恢复数据库或使其还原文件或文件组，请使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 语句或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Restore** 任务。 有关详细信息，请参阅[还原和恢复 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)。  
  
##  <a name="Additional_Considerations"></a> 有关 BACKUP 选项的其他注意事项  
  
###  <a name="Interactions_SKIP_etc"></a> SKIP、NOSKIP、INIT 和 NOINIT 之间的交互  
 下表说明了 { **NOINIT** | INIT } and { **NOSKIP** | SKIP } 选项之间的交互。  
  
> [!NOTE]  
> 如果磁带介质为空或磁盘备份文件不存在，则所有这些交互将写入介质标头并继续进行。 如果介质不为空但缺少有效的介质标头，则这些操作将反馈相关信息，指出这是无效的 MTF 介质，然后终止备份操作。  
  
||NOINIT|INIT|  
|------|------------|----------|  
|NOSKIP|如果卷中包含有效的介质标头，则验证介质名称是否匹配给定的 `MEDIANAME`（如果有）。 如果匹配，则追加备份集，同时保留所有现有的备份集。<br /> 如果卷中不含有效的介质标头，则会发生错误。|如果卷中包含有效的介质标头，将执行以下检查：<br /><ul><li>如果指定了 `MEDIANAME`，则验证给定的介质名称是否匹配介质标头的介质名称。<sup>1</sup></li><li>确保介质上没有未过期的备份集。 如果有，则终止备份。</li></ul><br />如果这些检查都通过了，则覆盖该介质上的所有备份集，只保留介质标头。<br /> 如果卷中不含有效的介质标头，则使用指定的 `MEDIANAME` 和 `MEDIADESCRIPTION`（如果有）生成一个介质标头。|  
|SKIP|如果卷中包含有效的介质标头，则追加备份集，并保留所有现有备份集。|如果卷中包含有效的<sup>2</sup> 介质标头，则覆盖介质上的所有备份集，仅保留介质标头。<br /> 如果介质为空，则使用指定的 `MEDIANAME` 和 `MEDIADESCRIPTION`（如果有）生成一个介质标头。|  

<sup>1</sup> 用户必须属于相应的固定数据库或服务器角色才能执行备份操作。    

<sup>2</sup> 有效性包括 MTF 版本号和其他标头信息。 如果不支持指定的版本或指定的版本不是期望值，将会发生错误。  
  
## <a name="compatibility"></a>兼容性  
  
> [!CAUTION]  
> 无法在早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中还原较新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]创建的备份。  
  
BACKUP 支持 `RESTART` 选项以提供与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本的向后兼容性。 但是，RESTART 不起作用。  
  
## <a name="general-remarks"></a>一般备注  
可以将数据库或日志备份追加到任何磁盘或磁带设备上，从而将数据库及其事务日志保存在一个物理位置中。  
  
不允许在显式或隐式事务中使用 BACKUP 语句。  
  
只要操作系统支持数据库的排序规则，就可以在不同的平台之间执行备份操作，即使这些平台使用不同的处理器类型。  
 
对具有单个数据文件且支持[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) 的数据库使用备份压缩时，建议使用大于 65536 (64 KB) 的 `MAXTRANSFERSIZE` 设置。   
从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，这为 TDE 加密数据库启用优化的压缩算法，该算法先解密页面，然后将其压缩并再次对其进行加密。 如果使用 `MAXTRANSFERSIZE = 65536` (64 KB)，对使用 TDE 加密的数据库执行备份压缩时会直接压缩加密的页面，且可能不会得到良好的压缩比率。 有关详细信息，请参阅[支持 TDE 的数据库的备份压缩](http://blogs.msdn.microsoft.com/sqlcat/2016/06/20/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases/)。

> [!NOTE]  
> 某些情况下，默认的 `MAXTRANSFERSIZE` 大于 64K：
> * 数据库创建了多个数据文件时，它使用 `MAXTRANSFERSIZE` > 64K
> * 执行 URL 备份时，默认 `MAXTRANSFERSIZE = 1048576` (1MB)
>   
> 即使其中一个条件适用，也必须在备份命令中显式设置 `MAXTRANSFERSIZE` 大于 64K，才能获得新的备份压缩算法。
  
默认情况下，每个成功的备份操作都会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和系统事件日志中添加一个条目。 如果非常频繁地备份日志，这些成功消息会迅速累积，从而产生一个巨大的错误日志，这样会使查找其他消息变得非常困难。 在这些情况下，如果任何脚本均不依赖于这些日志条目，则可以使用跟踪标志 3226 取消这些条目。 有关详细信息，请参阅[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。  
  
## <a name="interoperability"></a>互操作性  
 在数据库仍在使用时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用联机备份过程对数据库进行备份。 在备份过程中，可以进行多个操作；例如：在执行备份操作期间允许使用 INSERT、UPDATE 或 DELETE 语句。  
  
 在数据库或事务日志备份的过程中无法运行的操作包括：  
  
-   文件管理操作，例如带有 `ADD FILE` 或 `REMOVE FILE` 选项的 `ALTER DATABASE` 语句。  
  
-   收缩数据库或文件操作。 这包括自动收缩操作。  
  
如果备份操作与文件管理操作或收缩操作重叠，则产生冲突。 无论哪个冲突操作先行开始，第二个操作总会等待第一个操作设置的锁超时（超时期限由会话超时设置控制）。 如果在超时期限内释放锁，第二个操作将继续执行。 如果锁超时，则第二个操作失败。  

## <a name="limitations-for-sql-database-managed-instance"></a>SQL 数据库托管实例的限制
SQL 数据库托管实例可将数据库备份为最多具有 32 个带状线的备份，如果使用了备份压缩，这对于 4 TB 的数据库都足够了。

最大备份带状线大小为 195 GB（最大 blob 大小）。 增加备份命令中的带状线数量以缩小单个带状线大小，将其保持在限制范围内。

> [!NOTE]
> 要解决本地上的此限制，可备份到 `DISK`，而不是备份到 `URL`，将备份文件上传到 blob，然后将其还原。 因为使用了不同的 blob 类型，所以还原可支持更大型的文件。

 
## <a name="metadata"></a>元数据  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包含以下备份历史记录表以跟踪备份活动：  
  
-   [backupfile (Transact-SQL)](../../relational-databases/system-tables/backupfile-transact-sql.md)  
-   [backupfilegroup (Transact-SQL)](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)  
-   [backupmediafamily (Transact-SQL)](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)  
-   [backupmediaset (Transact-SQL)](../../relational-databases/system-tables/backupmediaset-transact-sql.md)  
-   [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
在执行还原操作时，如果尚未在 msdb 数据库中记录备份集，则可以修改备份历史记录表。  
  
## <a name="security"></a>Security  
 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，`PASSWORD` 和 `MEDIAPASSWORD` 选项不可再用于创建备份。 仍可以还原使用密码创建的备份。  
  
### <a name="permissions"></a>权限  
 默认情况下，为 **sysadmin** 固定服务器角色以及 **db_owner** 和 **db_backupoperator** 固定数据库角色的成员授予 BACKUP DATABASE 和 BACKUP LOG 权限。  
  
 备份设备的物理文件的所有权和权限问题可能会妨碍备份操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须能够读取和写入设备；运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的帐户必须具有写入权限。 但是，用于在系统表中为备份设备添加项目的 [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)不检查文件访问权限。 备份设备物理文件的这些问题可能直到为备份或还原而访问物理资源时才会出现。  
  
##  <a name="examples"></a> 示例  
 本部分包含以下示例：  
  
-   A. [备份整个数据库](#backing_up_db)  
-   B. [备份数据库和日志](#backing_up_db_and_log)  
-   C. [创建辅助文件组的完整文件备份]
-   (#full_file_backup)  
-   D. [创建辅助文件组的差异文件备份](#differential_file_backup)  
-   E. [创建和备份到单簇镜像媒体集](#create_single_family_mirrored_media_set)  
-   F. [创建和备份到多簇镜像媒体集](#create_multifamily_mirrored_media_set)  
-   G [备份到现有镜像媒体集](#existing_mirrored_media_set)  
-   H. [在新的介质集中创建压缩备份](#creating_compressed_backup_new_media_set)  
-   I. [备份到 Microsoft Azure Blob 存储服务](#url)  
  
> [!NOTE]  
> 备份操作指南主题还包含其他示例。 有关详细信息，请参阅 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)。  
  
###  <a name="backing_up_db"></a> A. 备份整个数据库  
 以下示例将 [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] 数据库备份到磁盘文件。  
  
```sql  
BACKUP DATABASE AdventureWorks2012   
 TO DISK = 'Z:\SQLServerBackups\AdvWorksData.bak'  
   WITH FORMAT;  
GO  
```  
  
###  <a name="backing_up_db_and_log"></a> B. 备份数据库和日志  
 下面的示例备份 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库，默认情况下，该数据库使用简单恢复模式。 若要支持日志备份，请将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库改为使用完整恢复模式。  
  
 接下来，该示例使用 [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md) 创建一个逻辑[备份设备](../../relational-databases/backup-restore/backup-devices-sql-server.md)以备份数据 (`AdvWorksData`)，并创建另一个逻辑备份设备以备份日志 (`AdvWorksLog`)。  
  
 然后，该示例对 `AdvWorksData` 创建完整数据库备份，并在一段更新活动过后将日志备份到 `AdvWorksLog`。  
  
```sql  
-- To permit log backups, before the full database backup, modify the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
   SET RECOVERY FULL;  
GO  
-- Create AdvWorksData and AdvWorksLog logical backup devices.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'AdvWorksData',   
'Z:\SQLServerBackups\AdvWorksData.bak';  
GO  
EXEC sp_addumpdevice 'disk', 'AdvWorksLog',   
'X:\SQLServerBackups\AdvWorksLog.bak';  
GO  
  
-- Back up the full AdventureWorks2012 database.  
BACKUP DATABASE AdventureWorks2012 TO AdvWorksData;  
GO  
-- Back up the AdventureWorks2012 log.  
BACKUP LOG AdventureWorks2012  
   TO AdvWorksLog;  
GO  
```  
  
> [!NOTE]  
>  对于生产数据库，需要定期备份日志。 应当经常进行日志备份，以提供足够的保护来防止数据丢失。  
  
###  <a name="full_file_backup"></a> C. 创建辅助文件组的完整文件备份  
 下面的示例将对两个辅助文件组中的各个文件创建完整文件备份。  
  
```sql  
--Back up the files in SalesGroup1:  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck';  
GO  
```  
  
###  <a name="differential_file_backup"></a> D. 创建辅助文件组的差异文件备份  
 下面的示例将对两个辅助文件组中的各个文件创建差异文件备份。  
  
```sql  
--Back up the files in SalesGroup1:  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck'  
   WITH   
      DIFFERENTIAL;  
GO  
```  
  
###  <a name="create_single_family_mirrored_media_set"></a> E. 创建和备份到单簇镜像介质集  
 下面的示例将创建包含一个介质簇和四个镜像的镜像介质集，并将 [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] 数据库备份到其中。  
  
```sql  
BACKUP DATABASE AdventureWorks2012  
TO TAPE = '\\.\tape0'  
MIRROR TO TAPE = '\\.\tape1'  
MIRROR TO TAPE = '\\.\tape2'  
MIRROR TO TAPE = '\\.\tape3'  
WITH  
   FORMAT,  
   MEDIANAME = 'AdventureWorksSet0';  
```  
  
###  <a name="create_multifamily_mirrored_media_set"></a> F. 创建和备份到多簇镜像介质集  
 下面的示例将创建镜像介质集，其中每个镜像包含两个介质簇。 然后将 [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] 数据库备份到这两个镜像中。  
  
```sql  
BACKUP DATABASE AdventureWorks2012  
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'  
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'  
WITH  
   FORMAT,  
   MEDIANAME = 'AdventureWorksSet1';  
```  
  
###  <a name="existing_mirrored_media_set"></a> G. 备份到现有镜像介质集  
 下面的示例将备份集追加到在前面的示例中创建的介质集上。  
  
```sql  
BACKUP LOG AdventureWorks2012  
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'  
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'  
WITH   
   NOINIT,  
   MEDIANAME = 'AdventureWorksSet1';  
```  
  
> [!NOTE]  
>  为清楚起见，此处显示默认的 NOINIT。  
  
###  <a name="creating_compressed_backup_new_media_set"></a> H. 在新的介质集中创建压缩备份  
 下面的示例格式化介质，并创建新的介质集，然后对 [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] 数据库执行压缩完整备份。  
  
```sql  
BACKUP DATABASE AdventureWorks2012 TO DISK='Z:\SQLServerBackups\AdvWorksData.bak'   
WITH   
   FORMAT,   
   COMPRESSION;  
```  

###  <a name="url"></a> I. 备份到 Microsoft Azure Blob 存储服务 
以下示例向 Microsoft Azure Blob 存储服务执行完整的 `Sales` 数据库备份。  存储帐户名称为 `mystorageaccount`。  容器名称为 `myfirstcontainer`。  已经创建具有读取、写入、删除和列表权限的存储访问策略。  已使用与存储访问策略相关联的共享访问签名创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 凭据 `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`。  有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份到 Windows Azure Blob 存储服务的详细信息，请参阅[使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)和 [SQL Server 备份到 URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)。

```sql  
BACKUP DATABASE Sales
TO URL = 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_20160726.bak'
WITH STATS = 5;
```

  
## <a name="see-also"></a>另请参阅  
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [结尾日志备份 (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [DBCC SQLPERF (Transact-SQL)](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [RESTORE LABELONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)   
 [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [sp_addumpdevice (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [sp_helpfile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sp_helpfilegroup (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [使用内存优化表的数据库的段落还原](../../relational-databases/in-memory-oltp/piecemeal-restore-of-databases-with-memory-optimized-tables.md)  
  
  
