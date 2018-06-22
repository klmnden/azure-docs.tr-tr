---
title: Oluşturma ve Azure SQL esnek veritabanı iş Transact-SQL (T-SQL) kullanarak yönetme | Microsoft Docs
description: Komut dosyaları ile esnek veritabanı iş aracı Transact-SQL (T-SQL) kullanarak birçok veritabanı arasında çalışır.
services: sql-database
author: jaredmoo
manager: craigg
ms.service: sql-database
ms.topic: article
ms.date: 06/14/2018
ms.author: jaredmoo
ms.openlocfilehash: fb6e4ebd635d8afa8e679ee5bb0f5646f28f887b
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313342"
---
# <a name="use-transact-sql-t-sql-to-create-and-manage-elastic-database-jobs"></a>Esnek veritabanı işleri oluşturmak ve yönetmek için Transact-SQL (T-SQL) kullanın

Bu makale, T-SQL kullanarak esnek iş ile çalışmaya başlamak için birçok örnek senaryolar sağlar.

Örneklerde [saklı yordamlar](#job-stored-procedures) ve [görünümleri](#job-views) bulunan [ *iş veritabanına*](elastic-jobs-overview.md#job-database).

Transact-SQL (T-SQL), yapılandırmak, çalıştırmak, işleri oluşturmak ve yönetmek için kullanılır. Esnek İş Aracısı oluşturma desteklenmiyor T-SQL, önce oluşturmanız gerekir böylece bir *esnek İş Aracısı* portalı kullanarak veya [PowerShell](elastic-jobs-powershell.md#create-the-elastic-job-agent).


## <a name="create-a-credential-for-job-execution"></a>İş yürütme için kimlik bilgisi oluşturma

Kimlik bilgisi komut dosyası yürütme, hedef veritabanlarına bağlanmak için kullanılır. Kimlik bilgisi başarıyla betik yürütmek için uygun izinleri, hedef grubu tarafından belirtilen veritabanlarını gerekir. Bir sunucu ve/veya havuzu hedef grup üyesi kullanırken, sunucu ve/veya havuzu genişletmesi iş yürütme zaman önce kimlik bilgilerini yenilemek kullanılacak ana kimlik bilgisini oluşturmak için yüksek oranda önerilir. Veritabanı kapsamlı kimlik bilgisi İş Aracısı veritabanı oluşturulur. Aynı kimlik bilgisi için kullanılan *bir oturum açma* ve *bir kullanıcı oturum açma veritabanı izinleri vermek için oturumu açma oluşturun* hedef veritabanlarında.


```sql
--Connect to the job database specified when creating the job agent

-- Create a db master key if one does not already exist, using your own password.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';  
  
-- Create a database scoped credential.  
CREATE DATABASE SCOPED CREDENTIAL myjobcred WITH IDENTITY = 'jobcred',
    SECRET = '<EnterStrongPasswordHere>'; 
GO

-- Create a database scoped credential for the master database of server1.
CREATE DATABASE SCOPED CREDENTIAL mymastercred WITH IDENTITY = 'mastercred',
    SECRET = '<EnterStrongPasswordHere>'; 
GO
```

## <a name="create-a-target-group-servers"></a>Bir hedef grup (Sunucuları) oluşturun

Aşağıdaki örnek, bir sunucu bir işin tüm veritabanlarında yürütmek gösterilmiştir.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:


```sql
-- Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC jobs.sp_add_target_group 'ServerGroup1'

-- Add a server target member
EXEC jobs.sp_add_target_group_member
'ServerGroup1',
@target_type = 'SqlServer',
@refresh_credential_name='mymastercred', --credential required to refresh the databases in server
@server_name='server1.database.windows.net'

--View the recently created target group and target group members
SELECT * FROM jobs.target_groups WHERE target_group_name='ServerGroup1';
SELECT * FROM jobs.target_group_members WHERE target_group_name='ServerGroup1';
```


## <a name="exclude-a-single-database"></a>Tek bir veritabanı Dışla

Aşağıdaki örnekte nasıl adlı veritabanı dışında bir sunucuyla bir işin tüm veritabanlarında yürütüleceği gösterilmektedir *MappingDB*.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC [jobs].sp_add_target_group = N'ServerGroup'
GO

-- Add a server target member
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'London.database.windows.net'
GO

-- Add a server target member
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name='server2.database.windows.net'
GO

--Excude a database target member from the server target group
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@membership_type = N'Exclude',
@target_type = N'SqlDatabase',
@server_name = N'server1.database.windows.net',
@database_name =N'MappingDB'
GO

--View the recently created target group and target group members
SELECT * FROM [jobs].target_groups WHERE target_group_name = N'ServerGroup';
SELECT * FROM [jobs].target_group_members WHERE target_group_name = N'ServerGroup';
```


## <a name="create-a-target-group-pools"></a>Bir hedef grup (havuzlar) oluşturun

Aşağıdaki örnek, bir veya daha fazla esnek havuzlar tüm veritabanlarına hedef gösterilmektedir.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing pool(s)
EXEC jobs.sp_add_target_group 'PoolGroup'

-- Add an elastic pool(s) target member
EXEC jobs.sp_add_target_group_member
'PoolGroup',
@target_type = 'SqlElasticPool',
@refresh_credential_name='mymastercred', --credential required to refresh the databases in server
@server_name='server1.database.windows.net',
@elastic_pool_name='ElasticPool-1'

-- View the recently created target group and target group members
SELECT * FROM jobs.target_groups WHERE target_group_name = N'PoolGroup';
SELECT * FROM jobs.target_group_members WHERE target_group_name = N'PoolGroup';
```


## <a name="deploy-new-schema-to-many-databases"></a>Birçok veritabanı için yeni şema dağıtma

Aşağıdaki örnek, tüm veritabanları için yeni şema dağıtma gösterilmektedir.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:


```sql
--Connect to the job database specified when creating the job agent

--Add job for create table
EXEC jobs.sp_add_job @job_name='CreateTableTest', @description='Create Table Test'

-- Add job step for create table
EXEC jobs.sp_add_jobstep @job_name='CreateTableTest',
@command=N'IF NOT EXISTS (SELECT * FROM sys.tables 
            WHERE object_id = object_id(''Test''))
CREATE TABLE [dbo].[Test]([TestId] [int] NOT NULL);',
@credential_name='myjobcred',
@target_group_name='PoolGroup'
```


## <a name="data-collection-using-built-in-parameters"></a>Yerleşik parametrelerini kullanarak veri toplama

Birçok veri toplama senaryolarda bazı iş sonuçlarını işlem sonrası yardımcı olması için bu komut dosyası değişkenleri eklemek yararlı olabilir.

- $(job_name)
- $(job_id)
- $(job_version)
- $(step_id)
- $(step_name)
- $(job_execution_id)
- $(job_execution_create_time)
- $(target_group_name)

Örneğin, tüm birlikte aynı iş yürütme sonuçları gruplandırmak için kullanmak *$(job_execution_id)* aşağıdaki komutta gösterildiği gibi:


```sql
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());'
```


## <a name="monitor-database-performance"></a>Veritabanı performansını izleme

Aşağıdaki örnek, birden çok veritabanlarından performans verilerini toplamak için yeni bir proje oluşturur.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutları çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Add a job to collect perf results
EXEC jobs.sp_add_job @job_name ='ResultsJob', @description='Collection Performance data from all customers'

-- Add a job step w/ schedule to collect results
EXEC jobs.sp_add_jobstep
@job_name='ResultsJob',
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());',
@credential_name='myjobcred',
@target_group_name='PoolGroup',
@output_type='SqlDatabase',
@output_credential_name=’myjobcred’,
@output_server_name=’server1.database.windows.net',
@output_database_name=’<resultsdb>',
@output_table_name='<resutlstable>'
Create a job to monitor pool performance
--Connect to the job database specified when creating the job agent

-- Add a target group containing master database
EXEC jobs.sp_add_target_group 'MasterGroup'

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name='MasterGroup',
@target_type='SqlDatabase',
@server_name='server1.database.windows.net',
@database_name='master'

-- Add a job to collect perf results
EXEC jobs.sp_add_job
@job_name='ResultsPoolsJob',
@description='Demo: Collection Performance data from all pools',
@schedule_interval_type='Minutes',
@schedule_interval_count=15

-- Add a job step w/ schedule to collect results
EXEC jobs.sp_add_jobstep
@job_name='ResultsPoolsJob',
@command=N'declare @now datetime
DECLARE @startTime datetime
DECLARE @endTime datetime
DECLARE @poolLagMinutes datetime
DECLARE @poolStartTime datetime
DECLARE @poolEndTime datetime
SELECT @now = getutcdate ()
SELECT @startTime = dateadd(minute, -15, @now)
SELECT @endTime = @now
SELECT @poolStartTime = dateadd(minute, -30, @startTime)
SELECT @poolEndTime = dateadd(minute, -30, @endTime)

SELECT elastic_pool_name , end_time, elastic_pool_dtu_limit, avg_cpu_percent, avg_data_io_percent, avg_log_write_percent, max_worker_percent, max_session_percent,
        avg_storage_percent, elastic_pool_storage_limit_mb FROM sys.elastic_pool_resource_stats
        WHERE end_time > @poolStartTime and end_time <= @poolEndTime;
'),
@credential_name='myjobcred',
@target_group_name='MasterGroup',
@output_type='SqlDatabase',
@output_credential_name='myjobcred',
@output_server_name=’server1.database.windows.net',
@output_database_name=’resultsdb',
@output_table_name='resutlstable'
```


## <a name="view-job-definitions"></a>Görünüm iş tanımları

Aşağıdaki örnek, geçerli iş tanımları görüntülemek gösterilmiştir.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- View all jobs
SELECT * FROM jobs.jobs

-- View the steps of the current version of all jobs
SELECT js.* FROM jobs.jobsteps js
JOIN jobs.jobs j 
  ON j.job_id = js.job_id AND j.job_version = js.job_version

-- View the steps of all versions of all jobs
select * from jobs.jobsteps
```


## <a name="begin-ad-hoc-execution-of-a-job"></a>Bir işin geçici yürütülmesine başlar

Aşağıdaki örnek, bir işi hemen başlatmak gösterilmiştir.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Execute the latest version of a job
EXEC jobs.sp_start_job 'CreateTableTest'

-- Execute the latest version of a job and receive the execution id
declare @je uniqueidentifier
exec jobs.sp_start_job 'CreateTableTest', @job_execution_id = @je output
select @je

select * from jobs.job_executions where job_execution_id = @je

-- Execute a specific version of a job (e.g. version 1)
exec jobs.sp_start_job 'CreateTableTest', 1
```


## <a name="schedule-execution-of-a-job"></a>Bir işin zamanlaması yürütme

Aşağıdaki örnek, gelecekteki yürütme için bir işi zamanlama gösterilmektedir.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_update_job
@job_name='ResultsJob',
@enabled=1,
@schedule_interval_type='Minutes',
@schedule_interval_count=15
```

## <a name="monitor-job-execution-status"></a>İş yürütme durumu izleme

Aşağıdaki örnek, tüm işleri yürütme durumu ayrıntılarını görüntülemek gösterilmiştir.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

--View top-level execution status for the job named ‘ResultsPoolJob’
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' and step_id IS NULL
ORDER BY start_time DESC

--View all top-level execution status for all jobs
SELECT * FROM jobs.job_executions WHERE step_id IS NULL
ORDER BY start_time DESC

--View all execution statuses for job named ‘ResultsPoolsJob’
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' 
ORDER BY start_time DESC

-- View all active executions
SELECT * FROM jobs.job_executions 
WHERE is_active = 1
ORDER BY start_time DESC
```


## <a name="cancel-a-job"></a>Bir işi iptal etme

Aşağıdaki örnek, bir işi iptal gösterilmektedir.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- View all active executions to determine job execution id
SELECT * FROM jobs.job_executions 
WHERE is_active = 1 AND job_name = 'ResultPoolsJob'
ORDER BY start_time DESC
GO

-- Cancel job execution with the specified job execution id
EXEC jobs.sp_stop_job '01234567-89ab-cdef-0123-456789abcdef'
```


## <a name="delete-old-job-history"></a>Eski iş geçmişini sil

Aşağıdaki örnek, belirli bir tarihten önce iş geçmişini sil gösterilmektedir.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Delete history of a specific job’s executions older than the specified date
EXEC jobs.sp_purge_jobhistory @job_name='ResultPoolsJob', @oldest_date='2016-07-01 00:00:00'

--Note: job history is automatically deleted if it is >45 days old
```

## <a name="delete-a-job-and-all-its-job-history"></a>Bir iş ve tüm iş geçmişini sil

Aşağıdaki örnek bir işi silmek nasıl gösterir ve tüm iş geçmişi ilgili.  
Bağlanmak [ *iş veritabanına* ](elastic-jobs-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_delete_job @job_name='ResultsPoolsJob'

--Note: job history is automatically deleted if it is >45 days old
```




## <a name="job-stored-procedures"></a>İş saklı yordamlar

Aşağıdaki saklı yordamları bulunan [işleri veritabanı](elastic-jobs-overview.md#job-database).



|Saklı yordam  |Açıklama  |
|---------|---------|
|[sp_add_job](#spaddjob)     |     Yeni bir iş ekler.    |
|[sp_update_job ](#spupdatejob)    |      Varolan bir projeyi güncelleştirir.   |
|[sp_delete_job](#spdeletejob)     |      Var olan bir işi siler.   |
|[sp_add_jobstep](#spaddjobstep)    |    Bir adımı bir projeye ekler.     |
|[sp_update_jobstep](#spupdatejobstep)     |     Bir iş adımı güncelleştirir.    |
|[sp_delete_jobstep](#spdeletejobstep)     |     Bir iş adımı siler.    |
|[sp_start_job](#spstartjob)    |  Bir işi yürütülüyor başlatır.       |
|[sp_stop_job](#spstopjob)     |     İş yürütme durdurur.   |
|[sp_add_target_group](#spaddtargetgroup)    |     Bir hedef grup ekler.    |
|[sp_delete_target_group](#spdeletetargetgroup)     |    Hedef grubunu siler.     |
|[sp_add_target_group_member](#spaddtargetgroupmember)     |    Bir veritabanı veya veritabanları grubunu bir hedef gruba ekler.     |
|[sp_delete_target_group_member](#spdeletetargetgroupmember)     |     Hedef grup üyesi hedef gruptan kaldırır.    |
|[sp_purge_jobhistory ](#sppurgejobhistory)    |    Bir iş geçmişi kayıtları kaldırır.     |





### <a name="spaddjob"></a>sp_add_job

Yeni bir iş ekler. 
  
#### <a name="syntax"></a>Sözdizimi  
  

```sql
[jobs].sp_add_job [ @job_name = ] 'job_name'  
    [ , [ @description = ] 'description' ]   
    [ , [ @enabled = ] enabled ]
    [ , [ @schedule_interval_type = ] schedule_interval_type ]  
    [ , [ @schedule_interval_count = ] schedule_interval_count ]   
    [ , [ @schedule_start_time = ] schedule_start_time ]   
    [ , [ @schedule_end_time = ] schedule_end_time ]   
    [ , [ @job_id = ] job_id OUTPUT ]
```

  
#### <a name="arguments"></a>Bağımsız Değişkenler  

[  **@job_name =** ] 'job_name'  
İş adı. Ad benzersiz olmalıdır ve yüzde (%) karakter içeremez. job_name varsayılan ile nvarchar(128) ' dir.

[  **@description =** ] 'description'  
İş açıklaması. Varsayılan değer null nvarchar(512) açıklamasıdır. Açıklama atlanırsa, boş bir dize kullanılır.

[  **@enabled =** ] etkin  
İş zamanlamasını etkinleştirilip etkinleştirilmediği. Etkin, 0 (devre dışı) bir varsayılan bit. 0 ise, iş etkin değil ve kendi zamanlamasına göre çalışmaz; Ancak, bu el ile çalıştırılabilir. 1 ise, iş, zamanlamaya göre çalışır ve aynı zamanda el ile çalıştırılabilir.

[  **@schedule_interval_type =**] schedule_interval_type  
İşin ne zaman yürütüleceğini değeri gösterir. schedule_interval_type kez varsayılan nvarchar(50) desteklenir ve aşağıdaki değerlerden biri olabilir:
- 'Bir kez'
- 'Minutes'
- 'Hours'
- 'Days'
- 'Hafta'
- 'Months'

[  **@schedule_interval_count =** ] schedule_interval_count  
Her iş yürütme arasında gerçekleşecek şekilde schedule_interval_count dönem sayısı. schedule_interval_count int, varsayılan değer 1 ' dir. Değer 1'e eşit veya daha büyük olmalıdır.

[  **@schedule_start_time =** ] schedule_start_time  
Hangi iş üzerinde yürütme başlayabileceğiniz tarih. schedule_start_time DATETIME2, 0001-01-01 00:00:00.0000000 varsayılan değer ' dir.

[  **@schedule_end_time =** ] schedule_end_time  
Tarih hangi iş üzerinde yürütme durdurabilirsiniz. schedule_end_time varsayılandır DATETIME2, 9999-12-31 ile 11:59:59.0000000. 

[  **@job_id =** ] job_id çıkış  
Projeye başarıyla oluşturulduysa atanan iş kimlik numarası. job_id türü uniqueidentifier bir çıktı değişkenidir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri

0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
sp_add_job İş Aracısı oluştururken, belirtilen İş Aracısı veritabanından çalıştırılması gerekir.
Bir proje eklemek için sp_add_job yürütüldükten sonra sp_add_jobstep işi etkinlikleri gerçekleştirmek adımları eklemek için kullanılabilir. İşin ilk sürüm numarası ilk adımı eklendiğinde 1 olarak arttırılır 0 ' dır.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:

- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.

### <a name="spupdatejob"></a>sp_update_job

Varolan bir projeyi güncelleştirir.

#### <a name="syntax"></a>Sözdizimi

```sql
[jobs].sp_update_job [ @job_name = ] 'job_name'  
    [ , [ @new_name = ] 'new_name' ]
    [ , [ @description = ] 'description' ]   
    [ , [ @enabled = ] enabled ]
    [ , [ @schedule_interval_type = ] schedule_interval_type ]  
    [ , [ @schedule_interval_count = ] schedule_interval_count ]   
    [ , [ @schedule_start_time = ] schedule_start_time ]   
    [ , [ @schedule_end_time = ] schedule_end_time ]   
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@job_name =** ] 'job_name'  
Güncelleştirilecek işinin adı. job_name nvarchar(128) ' dir.

[  **@new_name =** ] 'new_name'  
İş yeni adı. new_name nvarchar(128) ' dir.

[  **@description =** ] 'description'  
İş açıklaması. nvarchar(512) açıklamasıdır.

[  **@enabled =** ] etkin  
İş zamanlamasını etkin (1) olup olmadığını belirtir veya (0) etkin değil. Etkin bit.

[  **@schedule_interval_type=** ] schedule_interval_type  
İşin ne zaman yürütüleceğini değeri gösterir. schedule_interval_type nvarchar(50) desteklenir ve aşağıdaki değerlerden biri olabilir:

- 'Bir kez'
- 'Minutes'
- 'Hours'
- 'Days'
- 'Hafta'
- 'Months'

[  **@schedule_interval_count=** ] schedule_interval_count  
Her iş yürütme arasında gerçekleşecek şekilde schedule_interval_count dönem sayısı. schedule_interval_count int, varsayılan değer 1 ' dir. Değer 1'e eşit veya daha büyük olmalıdır.

[  **@schedule_start_time=** ] schedule_start_time  
Hangi iş üzerinde yürütme başlayabileceğiniz tarih. schedule_start_time DATETIME2, 0001-01-01 00:00:00.0000000 varsayılan değer ' dir.

[  **@schedule_end_time=** ] schedule_end_time  
Tarih hangi iş üzerinde yürütme durdurabilirsiniz. schedule_end_time varsayılandır DATETIME2, 9999-12-31 ile 11:59:59.0000000. 

#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Bir proje eklemek için sp_add_job yürütüldükten sonra sp_add_jobstep işi etkinlikleri gerçekleştirmek adımları eklemek için kullanılabilir. İşin ilk sürüm numarası ilk adımı eklendiğinde 1 olarak arttırılır 0 ' dır.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.



### <a name="spdeletejob"></a>sp_delete_job

Var olan bir işi siler.

#### <a name="syntax"></a>Sözdizimi

```sql
[jobs].sp_delete_job [ @job_name = ] 'job_name'
    [ , [ @force = ] force ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@job_name =** ] 'job_name'  
Silinecek iş adı. job_name nvarchar(128) ' dir.

[  **@force =** ] zorla  
İşi devam eden tüm yürütmeleri varsa silin ve tüm iş yürütmeleri (0) sürüyor durumunda tüm süren yürütmeleri (1) veya başarısız iptal belirtir. zorla bit.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Bir işi silindiğinde, iş geçmişi otomatik olarak silinir.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.



### <a name="spaddjobstep"></a>sp_add_jobstep

Bir adımı bir projeye ekler.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_add_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] step_name ]   
     [ , [ @command_type = ] 'command_type' ]   
     [ , [ @command_source = ] 'command_source' ]  
     , [ @command = ] 'command'
     , [ @credential_name = ] 'credential_name'
     , [ @target_group_name = ] 'target_group_name'
     [ , [ @initial_retry_interval_seconds = ] initial_retry_interval_seconds ]   
     [ , [ @maximum_retry_interval_seconds = ] maximum_retry_interval_seconds ]   
     [ , [ @retry_interval_backoff_multiplier = ] retry_interval_backoff_multiplier ]   
     [ , [ @retry_attempts = ] retry_attempts ]   
     [ , [ @step_timeout_seconds = ] step_timeout_seconds ]   
     [ , [ @output_type = ] 'output_type' ]   
     [ , [ @output_credential_name = ] 'output_credential_name' ]   
     [ , [ @output_subscription_id = ] 'output_subscription_id' ]   
     [ , [ @output_resource_group_name = ] 'output_resource_group_name' ]   
     [ , [ @output_server_name = ] 'output_server_name' ]   
     [ , [ @output_database_name = ] 'output_database_name' ]   
     [ , [ @output_schema_name = ] 'output_schema_name' ]   
     [ , [ @output_table_name = ] 'output_table_name' ]
     [ , [ @job_version = ] job_version OUTPUT ]
     [ , [ @max_parallelism = ] max_parallelism ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler

[  **@job_name =** ] 'job_name'  
Hangi adımı eklemek proje adı. job_name nvarchar(128) ' dir.

[  **@step_id =** ] step_id  
İşi adımı dizisi kimlik numarası. Adım kimlik numaraları 1'den başlar ve boşluk artırın. Var olan bir adım bu kimliği zaten varsa, böylece bu yeni adım dizisi eklenebilir sonra adım ve aşağıdaki adımların tümünü kendi kimliğine sahip oldukları artırılır. Belirtilmezse, step_id otomatik olarak adımları sırayla son atanır. step_id bir tamsayı.

[  **@step_name =** ] step_name  
Adım adı. , (İçin kullanışlı) 'İş' varsayılan adına sahip bir iş ilk adımı hariç belirtilmelidir. step_name nvarchar(128) ' dir.

[  **@command_type =** ] 'command_type'  
Bu iş tarafından yürütülen komutu türü. command_type olan değerini anlamı TSql varsayılan değerini nvarchar(50) @command_type bir T-SQL betiği bir parametredir.

Belirtilmişse, değerin TSql olması gerekir.

[  **@command_source =** ] 'command_source'  
Komut depolandığı konumun türü. command_source olan satır içi değerini anlamı, varsayılan değerini nvarchar(50) @command_source parametredir metin komutu.

Belirtilmişse, değerin satır içi olması gerekir.

[  **@command =** ] 'komutu'  
Komut geçerli T-SQL komut dosyası olması gerekir ve bu iş adımı tarafından yürütülür. Varsayılan değer null nvarchar(max) komuttur.

[  **@credential_name =** ] 'credential_name'  
Veritabanının adını bu adım çalıştırıldığında, her bir hedef grup içindeki hedef veritabanlarına bağlanmak için kullanılan bu işi denetim veritabanında depolanan kimlik bilgileri kapsamlı. credential_name nvarchar(128) ' dir.

[  **@target_group_name =** ] 'hedef-grup_adı'  
İşi adımı üzerinde yürütülen hedef veritabanlarını içeren hedef grubun adı. target_group_name nvarchar(128) ' dir.

[  **@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
İlk yeniden denemeden önce iş adımı ilk yürütme denemede başarısız olursa gecikmesi. initial_retry_interval_seconds int, varsayılan değeri 1 ' dir.

[  **@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
Yeniden deneme girişimleri arasındaki en büyük gecikme. Yeniden denemeler arasındaki gecikme bu değerden daha büyük büyümesine varsa, bu değeri bunun yerine tutulabilir. maximum_retry_interval_seconds int, varsayılan değer 120 ' dir.

[  **@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
Birden fazla iş yürütme adım için yeniden deneme gecikmesini uygulamak için çarpanı denemesi başarısız. Örneğin, ilk yeniden deneme gecikmesi 5 saniye olan ve geri Çekilme çarpanı 2.0 ise, ardından ikinci yeniden deneme gecikmesi 10 saniye sahip olur ve üçüncü yeniden deneme 20 saniye bir gecikme olacaktır. retry_interval_backoff_multiplier gerçek, 2.0 varsayılan değeri olan ' dir.

[  **@retry_attempts =** ] retry_attempts  
Yürütme ilk denemesi başarısız olursa yeniden deneneceğini sayısı. Olacaktır 1 ilk retry_attempts değeri 10 ise örneğin girişimini ve 10 11 denemeleri toplam vermiş, yeniden deneme sayısı. Son yeniden deneme girişimi başarısız olursa, iş yürütme başarısız bir ömrü sona erdirir. retry_attempts int, varsayılan değer 10 ' dir.

[  **@step_timeout_seconds =** ] step_timeout_seconds  
En uzun süreyi adımın yürütmek için izin verilir. Bu süre aşılırsa, iş yürütme süresi sona erdi yaşam döngüsü ile sona erdirir. step_timeout_seconds int, değer 43.200 saniye (12 saat) varsayılan değere sahip olur.

[  **@output_type =** ] 'output_type'  
Aksi halde null komutunun ilk sonuç kümesi hedef türü yazılır. output_type NULL varsayılan nvarchar(50) ' dir.

Belirtilmişse, değerin SqlDatabase olması gerekir.

[  **@output_credential_name =** ] 'output_credential_name'  
Boş değilse, veritabanının adını çıkış hedef veritabanına bağlanmak için kullanılan kimlik bilgisi kapsamlı. Output_type SqlDatabase eşitse belirtilmelidir. output_credential_name varsayılan değeri NULL ile nvarchar(128) ' dir.

[  **@output_subscription_id =** ] 'output_subscription_id'  
Açıklama gerekir.

[  **@output_resource_group_name =** ] 'output_resource_group_name'  
Açıklama gerekir.

[  **@output_server_name =** ] 'output_server_name'  
Aksi halde null, çıktı hedef veritabanını içeren sunucusunun tam DNS adı. Output_type SqlDatabase eşitse belirtilmelidir. output_server_name NULL varsayılan nvarchar(256) ' dir.

[  **@output_database_name =** ] 'output_database_name'  
Aksi halde null, çıktı hedef tablo içeren bir veritabanı adı. Output_type SqlDatabase eşitse belirtilmelidir. output_database_name NULL varsayılan nvarchar(128) ' dir.

[  **@output_schema_name =** ] 'output_schema_name'  
Aksi halde null, çıktı hedef tablosu içeren SQL şemasının adı. Output_type SqlDatabase eşitse, varsayılan değer dbo ' dir. output_schema_name nvarchar(128) ' dir.

[  **@output_table_name =** ] 'output_table_name'  
Aksi halde null komutunun ilk sonuç kümesi tablonun adı yazılır. Tablonun zaten yoksa, sonuç kümesi döndüren şema göre oluşturulur. Output_type SqlDatabase eşitse belirtilmelidir. output_table_name varsayılan değeri NULL ile nvarchar(128) ' dir.

[  **@job_version =** ] job_version çıkış  
Yeni iş sürüm numarasını atanacak çıktı parametresi. job_version tamsayı.

[  **@max_parallelism =** ] max_parallelism çıkış  
Paralellik esnek havuz başına maksimum düzeyi. Set sonra işi adımı yalnızca birçok veritabanı esnek havuz başına maksimum, üzerinde çalıştırmak için sınırlı olursa. Bu, ya da doğrudan hedef gruba dahil edildi veya hedef gruba dahil Sunucusu'ndaki her esnek havuz için geçerlidir. max_parallelism tamsayı.


#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Sp_add_jobstep başarılı olduğunda işin geçerli sürüm numarasını artırılır. İşin bir sonraki yürütüldüğünde yeni sürümü kullanılır. İş şu anda yürütülmekte varsa, o yürütme yeni adımı içermez.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:  

- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.



### <a name="spupdatejobstep"></a>sp_update_jobstep

Bir iş adımı güncelleştirir.

#### <a name="syntax"></a>Sözdizimi

```sql
[jobs].sp_update_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @new_id = ] new_id ]   
     [ , [ @new_name = ] 'new_name' ]   
     [ , [ @command_type = ] 'command_type' ]   
     [ , [ @command_source = ] 'command_source' ]  
     , [ @command = ] 'command'
     , [ @credential_name = ] 'credential_name'
     , [ @target_group_name = ] 'target_group_name'
     [ , [ @initial_retry_interval_seconds = ] initial_retry_interval_seconds ]   
     [ , [ @maximum_retry_interval_seconds = ] maximum_retry_interval_seconds ]   
     [ , [ @retry_interval_backoff_multiplier = ] retry_interval_backoff_multiplier ]   
     [ , [ @retry_attempts = ] retry_attempts ]   
     [ , [ @step_timeout_seconds = ] step_timeout_seconds ]   
     [ , [ @output_type = ] 'output_type' ]   
     [ , [ @output_credential_name = ] 'output_credential_name' ]   
     [ , [ @output_server_name = ] 'output_server_name' ]   
     [ , [ @output_database_name = ] 'output_database_name' ]   
     [ , [ @output_schema_name = ] 'output_schema_name' ]   
     [ , [ @output_table_name = ] 'output_table_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
     [ , [ @max_parallelism = ] max_parallelism ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@job_name =** ] 'job_name'  
Adım ait olduğu iş adı. job_name nvarchar(128) ' dir.

[  **@step_id =** ] step_id  
Değiştirilecek işi adımı kimlik numarası. Step_id veya step_name belirtilmelidir. step_id bir tamsayı.

[  **@step_name =** ] 'step_name'  
Değiştirilecek adımının adı. Step_id veya step_name belirtilmelidir. step_name nvarchar(128) ' dir.

[  **@new_id =** ] new_id  
Yeni sıra kimlik numarası işi adımı. Adım kimlik numaraları 1'den başlar ve boşluk artırın. Bir adımı kaldırılmasında, diğer adımları otomatik olarak numaralandırılır.

[  **@new_name =** ] 'new_name'  
Adım yeni adı. new_name nvarchar(128) ' dir.

[  **@command_type =** ] 'command_type'  
Bu iş tarafından yürütülen komutu türü. command_type olan değerini anlamı TSql varsayılan değerini nvarchar(50) @command_type bir T-SQL betiği bir parametredir.

Belirtilmişse, değerin TSql olması gerekir.

[  **@command_source =** ] 'command_source'  
Komut depolandığı konumun türü. command_source olan satır içi değerini anlamı, varsayılan değerini nvarchar(50) @command_source parametredir metin komutu.

Belirtilmişse, değerin satır içi olması gerekir.

[  **@command =** ] 'komutu'  
Komutlar geçerli T-SQL komut dosyası olması gerekir ve daha sonra bu işi adımı tarafından çalıştırılabilir. Varsayılan değer null nvarchar(max) komuttur.

[  **@credential_name =** ] 'credential_name'  
Veritabanının adını bu adım çalıştırıldığında, her bir hedef grup içindeki hedef veritabanlarına bağlanmak için kullanılan bu işi denetim veritabanında depolanan kimlik bilgileri kapsamlı. credential_name nvarchar(128) ' dir.

[  **@target_group_name =** ] 'hedef-grup_adı'  
İşi adımı üzerinde yürütülen hedef veritabanlarını içeren hedef grubun adı. target_group_name nvarchar(128) ' dir.

[  **@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
İlk yeniden denemeden önce iş adımı ilk yürütme denemede başarısız olursa gecikmesi. initial_retry_interval_seconds int, varsayılan değeri 1 ' dir.

[  **@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
Yeniden deneme girişimleri arasındaki en büyük gecikme. Yeniden denemeler arasındaki gecikme bu değerden daha büyük büyümesine varsa, bu değeri bunun yerine tutulabilir. maximum_retry_interval_seconds int, varsayılan değer 120 ' dir.

[  **@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
Birden fazla iş yürütme adım için yeniden deneme gecikmesini uygulamak için çarpanı denemesi başarısız. Örneğin, ilk yeniden deneme gecikmesi 5 saniye olan ve geri Çekilme çarpanı 2.0 ise, ardından ikinci yeniden deneme gecikmesi 10 saniye sahip olur ve üçüncü yeniden deneme 20 saniye bir gecikme olacaktır. retry_interval_backoff_multiplier gerçek, 2.0 varsayılan değeri olan ' dir.

[  **@retry_attempts =** ] retry_attempts  
Yürütme ilk denemesi başarısız olursa yeniden deneneceğini sayısı. Olacaktır 1 ilk retry_attempts değeri 10 ise örneğin girişimini ve 10 11 denemeleri toplam vermiş, yeniden deneme sayısı. Son yeniden deneme girişimi başarısız olursa, iş yürütme başarısız bir ömrü sona erdirir. retry_attempts int, varsayılan değer 10 ' dir.

[  **@step_timeout_seconds =** ] step_timeout_seconds  
En uzun süreyi adımın yürütmek için izin verilir. Bu süre aşılırsa, iş yürütme süresi sona erdi yaşam döngüsü ile sona erdirir. step_timeout_seconds int, değer 43.200 saniye (12 saat) varsayılan değere sahip olur.

[  **@output_type =** ] 'output_type'  
Aksi halde null komutunun ilk sonuç kümesi hedef türü yazılır. Bu parametrenin değeri NULL olarak output_type değerini sıfırlamak için kümesine '' (boş dize). output_type NULL varsayılan nvarchar(50) ' dir.

Belirtilmişse, değerin SqlDatabase olması gerekir.

[  **@output_credential_name =** ] 'output_credential_name'  
Boş değilse, veritabanının adını çıkış hedef veritabanına bağlanmak için kullanılan kimlik bilgisi kapsamlı. Output_type SqlDatabase eşitse belirtilmelidir. Bu parametrenin değeri NULL olarak output_credential_name değerini sıfırlamak için kümesine '' (boş dize). output_credential_name varsayılan değeri NULL ile nvarchar(128) ' dir.

[  **@output_server_name =** ] 'output_server_name'  
Aksi halde null, çıktı hedef veritabanını içeren sunucusunun tam DNS adı. Output_type SqlDatabase eşitse belirtilmelidir. Bu parametrenin değeri NULL olarak output_server_name değerini sıfırlamak için kümesine '' (boş dize). output_server_name NULL varsayılan nvarchar(256) ' dir.

[  **@output_database_name =** ] 'output_database_name'  
Aksi halde null, çıktı hedef tablo içeren bir veritabanı adı. Output_type SqlDatabase eşitse belirtilmelidir. Bu parametrenin değeri NULL olarak output_database_name değerini sıfırlamak için kümesine '' (boş dize). output_database_name NULL varsayılan nvarchar(128) ' dir.

[  **@output_schema_name =** ] 'output_schema_name'  
Aksi halde null, çıktı hedef tablosu içeren SQL şemasının adı. Output_type SqlDatabase eşitse, varsayılan değer dbo ' dir. Bu parametrenin değeri NULL olarak output_schema_name değerini sıfırlamak için kümesine '' (boş dize). output_schema_name nvarchar(128) ' dir.

[  **@output_table_name =** ] 'output_table_name'  
Aksi halde null komutunun ilk sonuç kümesi tablonun adı yazılır. Tablonun zaten yoksa, sonuç kümesi döndüren şema göre oluşturulur. Output_type SqlDatabase eşitse belirtilmelidir. Bu parametrenin değeri NULL olarak output_server_name değerini sıfırlamak için kümesine '' (boş dize). output_table_name varsayılan değeri NULL ile nvarchar(128) ' dir.

[  **@job_version =** ] job_version çıkış  
Yeni iş sürüm numarasını atanacak çıktı parametresi. job_version tamsayı.

[  **@max_parallelism =** ] max_parallelism çıkış  
Paralellik esnek havuz başına maksimum düzeyi. Set sonra işi adımı yalnızca birçok veritabanı esnek havuz başına maksimum, üzerinde çalıştırmak için sınırlı olursa. Bu, ya da doğrudan hedef gruba dahil edildi veya hedef gruba dahil Sunucusu'ndaki her esnek havuz için geçerlidir. Max_parallelism değerini geri null olarak sıfırlamak için bu parametrenin değeri -1 olarak ayarlayın. max_parallelism tamsayı.


#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Tüm Süren yürütmeleri işin etkilenmez. Sp_update_jobstep başarılı olduğunda işin sürüm numarası artırılır. İşin bir sonraki yürütüldüğünde yeni sürümü kullanılır.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:

- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri diğer kullanıcılara ait işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz




### <a name="spdeletejobstep"></a>sp_delete_jobstep

Bir iş adımı bir işten kaldırır.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_delete_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@job_name =** ] 'job_name'  
Adım kaldırılacak işinin adı. job_name varsayılan ile nvarchar(128) ' dir.

[  **@step_id =** ] step_id  
Silinecek kimlik numarası işi adımı. Step_id veya step_name belirtilmelidir. step_id bir tamsayı.

[  **@step_name =** ] 'step_name'  
Silinecek adımının adı. Step_id veya step_name belirtilmelidir. step_name nvarchar(128) ' dir.

[  **@job_version =** ] job_version çıkış  
Yeni iş sürüm numarasını atanacak çıktı parametresi. job_version tamsayı.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Tüm Süren yürütmeleri işin etkilenmez. Sp_update_jobstep başarılı olduğunda işin sürüm numarası artırılır. İşin bir sonraki yürütüldüğünde yeni sürümü kullanılır.

Başka bir iş adımı, silinen iş adımı tarafından sol boşluğu doldurmak için otomatik olarak numaralandırılır.
 
#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.






### <a name="spstartjob"></a>sp_start_job

Bir işi yürütülüyor başlatır.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_start_job [ @job_name = ] 'job_name'   
     [ , [ @job_execution_id = ] job_execution_id OUTPUT ]   
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@job_name =** ] 'job_name'  
Adım kaldırılacak işinin adı. job_name varsayılan ile nvarchar(128) ' dir.

[  **@job_execution_id =** ] job_execution_id çıkış  
Çıkış iş yürütme ait kimlik atanacak parametresi. job_version uniqueidentifier ' dir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Yok.
 
#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.

### <a name="spstopjob"></a>sp_stop_job

İş yürütme durdurur.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_stop_job [ @job_execution_id = ] ' job_execution_id '
```


#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@job_execution_id =** ] job_execution_id  
Durdurmak için iş yürütme kimliği sayısı. job_execution_id varsayılan null ile uniqueidentifier ' dir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Yok.
 
#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.


### <a name="spaddtargetgroup"></a>sp_add_target_group

Bir hedef grup ekler.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_add_target_group [ @target_group_name = ] 'target_group_name'   
     [ , [ @target_group_id = ] target_group_id OUTPUT ]
```


#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@target_group_name =** ] 'target_group_name'  
Oluşturmak için hedef grup adı. target_group_name varsayılan ile nvarchar(128) ' dir.

[  **@target_group_id =** ] target_group_id çıkış hedef grup başarıyla oluşturulduysa projeye atanan kimlik numarası. target_group_id türü uniqueidentifier NULL varsayılan bir çıktı değişken değildir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Hedef grupları veritabanları koleksiyonunu bir işi hedeflemek için kolay bir yol sağlar.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.

### <a name="spdeletetargetgroup"></a>sp_delete_target_group

Hedef grubunu siler.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_delete_target_group [ @target_group_name = ] 'target_group_name'
```


#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@target_group_name =** ] 'target_group_name'  
Silmek için hedef grup adı. target_group_name varsayılan ile nvarchar(128) ' dir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Yok.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.

### <a name="spaddtargetgroupmember"></a>sp_add_target_group_member

Bir veritabanı veya veritabanları grubunu bir hedef gruba ekler.

#### <a name="syntax"></a>Sözdizimi

```sql
[jobs].sp_add_target_group_member [ @target_group_name = ] 'target_group_name'
         [ @membership_type = ] ‘membership_type’ ]   
        [ , [ @target_type = ] ‘target_type’ ]   
        [ , [ @refresh_credential_name = ] ‘refresh_credential_name’ ]   
        [ , [ @server_name = ] ‘server_name’ ]   
        [ , [ @database_name = ] ‘database_name’ ]   
        [ , [ @elastic_pool_name = ] ‘elastic_pool_name’ ]   
        [ , [ @shard_map_name = ] ‘shard_map_name’ ]   
        [ , [ @target_id = ] ‘target_id’ OUTPUT ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@target_group_name =** ] 'target_group_name'  
Üye eklenecek hedef grubun adı. target_group_name varsayılan ile nvarchar(128) ' dir.

[  **@membership_type =** ] 'membership_type'  
Hedef grup üyesi dahil etmek veya hariç olmadığını belirtir. 'Include' varsayılan ile nvarchar(128) target_group_name olur. 'Include' veya 'Dışla' target_group_name için geçerli değerler.

[  **@target_type =** ] 'target_type'  
Hedef veritabanı veya sunucu tüm veritabanları, esnek havuzdaki tüm veritabanları, bir parça eşlemindeki tüm veritabanları veya tek bir veritabanının dahil olmak üzere veritabanları koleksiyonunu türü. target_type varsayılan ile nvarchar(128) ' dir. 'SqlServer', 'SqlElasticPool', 'SqlDatabase' veya 'SqlShardMap' target_type için geçerli değerler. 

[  **@refresh_credential_name =** ] 'refresh_credential_name'  
Mantıksal sunucunun adıdır. refresh_credential_name varsayılan ile nvarchar(128) ' dir.

[  **@server_name =** ] 'sunucu_adı'  
Belirtilen hedef grubuna eklenmelidir mantıksal sunucunun adıdır. target_type 'SqlServer' olduğunda sunucu_adı belirtilmesi gerekir. sunucu_adı varsayılan ile nvarchar(128) ' dir.

[  **@database_name =** ] 'database_name'  
Belirtilen hedef grubuna eklenmelidir veritabanının adı. target_type 'SqlDatabase' olduğunda database_name belirtilmesi gerekir. database_name varsayılan ile nvarchar(128) ' dir.

[  **@elastic_pool_name =** ] 'elastic_pool_name'  
Belirtilen hedef grubuna eklenmelidir esnek havuz adı. target_type 'SqlElasticPool' olduğunda elastic_pool_name belirtilmesi gerekir. elastic_pool_name varsayılan ile nvarchar(128) ' dir.

[  **@shard_map_name =** ] 'shard_map_name'  
Belirtilen hedef grubuna eklenmelidir parça eşleme havuzunun adı. target_type 'SqlSqlShardMap' olduğunda elastic_pool_name belirtilmesi gerekir. shard_map_name varsayılan ile nvarchar(128) ' dir.

[  **@target_id =** ] target_group_id çıkış  
Hedef grup üyesi oluşturduysanız, atanan hedef kimlik numarası hedef grubuna eklendi. target_id türü uniqueidentifier NULL varsayılan bir çıktı değişken değildir.
Dönüş kodu değerler 0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Bir işin bir sunucu içindeki tüm veritabanlarında yürütür veya yürütme, bir mantıksal sunucu veya esnek havuz zamanında esnek havuz hedef gruba dahil edildi.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.

#### <a name="examples"></a>Örnekler
Aşağıdaki örnekte tüm veritabanları Londra ve NewYork sunucularını sunucuları koruma müşteri bilgileri grubuna ekler. İş Aracısı bu servis talebi ElasticJobs oluşturulurken belirtilen işleri veritabanına bağlanmalısınız.

```sql
--Connect to the jobs database specified when creating the job agent
USE ElasticJobs ; 
GO

-- Add a target group containing server(s)
EXEC jobs.sp_add_target_group @target_group_name =  N'Servers Maintaining Customer Information'
GO

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'London.database.windows.net' ;
GO

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'NewYork.database.windows.net' ;
GO

--View the recently added members to the target group
SELECT * FROM [jobs].target_group_members WHERE target_group_name= N'Servers Maintaining Customer Information';
GO
```

### <a name="spdeletetargetgroupmember"></a>sp_delete_target_group_member

Hedef grup üyesi hedef gruptan kaldırır.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_delete_target_group_member [ @target_group_name = ] 'target_group_name'
        [ , [ @target_id = ] ‘target_id’]
```



Bağımsız değişkenler [ @target_group_name =] 'target_group_name'  
Hedef grup üyesi kaldırmak için hedef grup adı. target_group_name varsayılan ile nvarchar(128) ' dir.

[ @target_id =] target_id  
 Kaldırılacak hedef grubu üyesi için atanan hedef kimlik numarası. target_id NULL varsayılan bir uniqueidentifier ' dir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Hedef grupları veritabanları koleksiyonunu bir işi hedeflemek için kolay bir yol sağlar.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.

#### <a name="examples"></a>Örnekler
Aşağıdaki örnek, Londra sunucunun sunucuları koruma müşteri bilgileri gruptan kaldırır. İş Aracısı bu servis talebi ElasticJobs oluşturulurken belirtilen işleri veritabanına bağlanmalısınız.

```sql
--Connect to the jobs database specified when creating the job agent
USE ElasticJobs ; 
GO

-- Retrieve the target_id for a target_group_members
declare @tid uniqueidentifier
SELECT @tid = target_id FROM [jobs].target_group_members WHERE target_group_name = 'Servers Maintaining Customer Information' and server_name = 'London.database.windows.net'

-- Remove a target group member of type server
EXEC jobs.sp_delete_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_id = @tid
GO
```

### <a name="sppurgejobhistory"></a>sp_purge_jobhistory

Bir iş geçmişi kayıtları kaldırır.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_purge_jobhistory [ @job_name = ] 'job_name'   
      [ , [ @job_id = ] job_id ]
      [ , [ @oldest_date = ] oldest_date []
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **@job_name =** ] 'job_name'  
İş Geçmişi kayıtları silmek için adı. job_name NULL varsayılan nvarchar(128) ' dir. Job_id veya job_name belirtilmesi gerekir, ancak her ikisi de belirtilemez.

[  **@job_id =** ] job_id  
 İş kimliği Silinecek kayıtlar için iş sayısı. job_id NULL varsayılan uniqueidentifier ' dir. Job_id veya job_name belirtilmesi gerekir, ancak her ikisi de belirtilemez.

[  **@oldest_date =** ] oldest_date  
 Geçmişte saklanacak eski kayıt. oldest_date DATETIME2, bir null ile varsayılandır. Oldest_date belirtildiğinde, sp_purge_jobhistory yalnızca belirtilen değerden daha eski kayıtları kaldırır.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
0 (başarılı) veya 1 (hata) Açıklamalar hedef grupları veritabanları koleksiyonunu bir işi hedeflemek için kolay bir yol sağlar.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Kısıtlama yalnızca bir kullanıcı işleri izleyebilmesi, İş Aracısı oluştururken, belirtilen İş Aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olarak kullanıcıya verebilirsiniz:
- jobs_reader

Bu rolleri izinleri hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılar tarafından sahip olunan işleri özniteliklerini düzenlemek için bu saklı yordamı kullanabilirsiniz.

#### <a name="examples"></a>Örnekler
Aşağıdaki örnekte tüm veritabanları Londra ve NewYork sunucularını sunucuları koruma müşteri bilgileri grubuna ekler. İş Aracısı bu servis talebi ElasticJobs oluşturulurken belirtilen işleri veritabanına bağlanmalısınız.

```sql
--Connect to the jobs database specified when creating the job agent

EXEC sp_delete_target_group_member   
    @target_group_name = N'Servers Maintaining Customer Information',  
    @server_name = N'London.database.windows.net';  
GO
```


## <a name="job-views"></a>İş görünümleri

Aşağıdaki görünümler kullanılabilir [işleri veritabanı](elastic-jobs-overview.md#job-database).


|Görünüm  |Açıklama  |
|---------|---------|
|[jobs_executions](#jobsexecutions-view)     |  Yürütme geçmişini gösterir işi.      |
|[İşlerini](#jobs-view)     |   Tüm işleri gösterir.      |
|[job_versions](#jobversions-view)     |   Tüm iş sürümleri gösterilir.      |
|[İş adımları](#jobsteps-view)     |     Tüm adımları her işi geçerli sürümde gösterir.    |
|[jobstep_versions](#jobstepversions-view)     |     Tüm adımları her işi tüm sürümlerini gösterir.    |
|[target_groups](#targetgroups-view)     |      Tüm hedef gruplarını gösterir.   |
|[target_group_members](#targetgroups-view)     |   Tüm hedef grupları tüm üyeleri gösterir.      |


### <a name="jobsexecutions-view"></a>jobs_executions görünümü

[işleri]. [jobs_executions]

Yürütme geçmişini gösterir işi.


|Sütun adı|   Veri türü   |Açıklama|
|---------|---------|---------|
|**job_execution_id**   |benzersiz tanımlayıcı|  İş yürütme örneği benzersiz kimliği.
|**job_name**   |Nvarchar(128)  |İş adı.
|**job_id** |benzersiz tanımlayıcı|  İşi benzersiz kimliği.
|**job_version**    |Int    |(İşini değiştiren her zaman otomatik olarak güncelleştirilir) iş sürümü.
|**step_id**    |Int|   Adım (için bu iş) benzersiz tanımlayıcısı. NULL, bu üst iş yürütme olduğunu gösterir.
|**is_active**| bit |Bilgi etkin veya devre dışı olup olmadığını gösterir. Etkin işlerin 1 gösterir ve 0 etkin olmayan gösterir.
|**Yaşam döngüsü**| nvarchar(50)|İşin durumunu gösteren değer: 'Oluşturulan', 'Sürüyor', 'Failed', 'Başarılı', 'Atlanan', 'SucceededWithSkipped'|
|**create_time**|   datetime2(7)|   İş oluşturulduğu tarih ve saat.
|**start_time** |datetime2(7)|  Tarih ve saat yürütme iş başlatıldı. İş henüz yürütülmedi yoksa NULL.
|**end_time**|  datetime2(7)    |Tarih ve saat iş yürütme tamamlandı. İş henüz yürütülmedi veya henüz yoksa null değerini henüz yürütme tamamlandı.
|**current_attempts**   |Int    |Adım yapılan yeniden deneme sayısı. Üst iş 0 olacaktır, büyük yürütme ilkesini temel alarak veya alt iş yürütmeleri 1 olacaktır.
|**current_attempt_start_time** |datetime2(7)|  Tarih ve saat yürütme iş başlatıldı. NULL, bu üst iş yürütme olduğunu gösterir.
|**last_message**   |nvarchar(max)| İş ya da adım Geçmiş iletisi. 
|**target_type**|   Nvarchar(128)   |Hedef veritabanı veya sunucu, esnek havuzdaki tüm veritabanları veya bir veritabanında tüm veritabanları da dahil olmak üzere veritabanları koleksiyonunu türü. 'SqlServer', 'SqlElasticPool' veya 'SqlDatabase' target_type için geçerli değerler. NULL, bu üst iş yürütme olduğunu gösterir.
|**target_id**  |benzersiz tanımlayıcı|  Hedef grup üyesi benzersiz kimliği.  NULL, bu üst iş yürütme olduğunu gösterir.
|**target_group_name**  |Nvarchar(128)  |Hedef grubun adı. NULL, bu üst iş yürütme olduğunu gösterir.
|**target_server_name**|    nvarchar(256)|  Hedef grupta bulunan mantıksal sunucunun adıdır. Yalnızca target_type 'SqlServer' kullanılıyorsa belirtilebilir. NULL, bu üst iş yürütme olduğunu gösterir.
|**target_database_name**   |Nvarchar(128)| Hedef grupta bulunan veritabanının adı. Yalnızca target_type 'SqlDatabase' olduğunda belirtildi. NULL, bu üst iş yürütme olduğunu gösterir.


### <a name="jobs-view"></a>işlerini görüntüleyin.

[işleri]. [işleri]

Tüm işleri gösterir.

|Sütun adı|   Veri türü|  Açıklama|
|------|------|-------|
|**job_name**|  Nvarchar(128)   |İş adı.|
|**job_id**|    benzersiz tanımlayıcı    |İşi benzersiz kimliği.|
|**job_version**    |Int    |(İşini değiştiren her zaman otomatik olarak güncelleştirilir) iş sürümü.|
|**Açıklama**    |nvarchar(512)| İş açıklaması. Etkin bit iş etkin mi yoksa devre dışı mı olduğunu belirtir. Etkin işlerin 1 gösterir ve 0, devre dışı işleri gösterir.|
|**schedule_interval_type** |nvarchar(50)   |İşin ne zaman yürütüleceğini gösteren değer: 'Bir kere', 'Minutes', 'Saat', 'Gün sayısı', ' hafta', 'Ay'
|**schedule_interval_count**|   Int|    Her iş yürütme arasında gerçekleşecek şekilde schedule_interval_type dönem sayısı.|
|**schedule_start_time**    |datetime2(7)|  Tarih ve saat iş son başlatılan yürütme.|
|**schedule_end_time**| datetime2(7)|   Tarih ve saat iş son tamamlanan yürütme.|


### <a name="jobversions-view"></a>job_versions görünümü

[işleri]. [job_verions]

Tüm iş sürümleri gösterilir.

|Sütun adı|   Veri türü|  Açıklama|
|------|------|-------|
|**job_name**|  Nvarchar(128)   |İş adı.|
|**job_id**|    benzersiz tanımlayıcı    |İşi benzersiz kimliği.|
|**job_version**    |Int    |(İşini değiştiren her zaman otomatik olarak güncelleştirilir) iş sürümü.|


### <a name="jobsteps-view"></a>İş adımları görünümü

[işleri]. [iş adımları]

Tüm adımları her işi geçerli sürümde gösterir.

|Sütun adı    |Veri türü| Açıklama|
|------|------|-------|
|**job_name**   |Nvarchar(128)| İş adı.|
|**job_id** |benzersiz tanımlayıcı   |İşi benzersiz kimliği.|
|**job_version**|   Int|    (İşini değiştiren her zaman otomatik olarak güncelleştirilir) iş sürümü.|
|**step_id**    |Int    |Adım (için bu iş) benzersiz tanımlayıcısı.|
|**step_name**  |Nvarchar(128)  |Adım (için bu iş) benzersiz adı.|
|**command_type**   |nvarchar(50)   |İş adımda yürütülecek komut türü. Değer v1 için eşit olmalıdır ve 'TSql' varsayılan değeri.|
|**command_source** |nvarchar(50)|  Komut dosyasının konumu. V1, 'Inline' varsayılandır ve yalnızca kabul edilen değer.|
|**komutu**|   nvarchar(max)|  Esnek iş command_type aracılığıyla tarafından çalıştırılacak komutları.|
|**credential_name**|   Nvarchar(128)   |İş yürütme için kullanılan veritabanı kapsamlı kimlik bilgisi adı.|
|**target_group_name**| Nvarchar(128)   |Hedef grubun adı.|
|**target_group_id**|   benzersiz tanımlayıcı|   Hedef grubun benzersiz kimliği.|
|**initial_retry_interval_seconds**|    Int |İlk yeniden deneme girişimi gecikme. Varsayılan değer 1'dir.|
|**maximum_retry_interval_seconds** |Int|   Yeniden deneme girişimleri arasındaki en büyük gecikme. Yeniden denemeler arasındaki gecikme bu değerden daha büyük büyümesine varsa, bu değeri bunun yerine tutulabilir. Varsayılan değer 120'dir.|
|**retry_interval_backoff_multiplier**  |Gerçek|  Birden fazla iş yürütme adım için yeniden deneme gecikmesini uygulamak için çarpanı denemesi başarısız. 2.0 varsayılan değerdir.|
|**retry_attempts** |Int|   Yeniden deneme sayısını, bu adım başarısız olursa kullanmayı dener. Yeniden deneme girişimleri gösteren varsayılan 10.|
|**step_timeout_seconds**   |Int|   Yeniden deneme girişimleri arasındaki dakika cinsinden süre miktarı. Varsayılan değer 0 dakikalık bir zaman aralığı gösteren 0 ' dır.|
|**output_type**    |nvarchar(11)|  Komut dosyasının konumu. Geçerli Önizleme 'Inline' varsayılandır ve yalnızca kabul edilen değer.|
|**output_credential_name**|    Nvarchar(128)   |Adı Sonuçların depolanacağı hedef sunucuya bağlanmak için kullanılacak kimlik bilgileri olarak ayarlayın.|
|**output_subscription_id**|    benzersiz tanımlayıcı|   Hedef server\database sonuçları sorgu yürütülmesini ayarlamak için aboneliği benzersiz kimliği.|
|**output_resource_group_name** |Nvarchar(128)| Kaynak grubu adı hedef sunucusunun bulunduğu.|
|**output_server_name**|    nvarchar(256)   |Sonuçları için hedef sunucunun adını ayarlayın.|
|**output_database_name**   |Nvarchar(128)| Sonuçları için hedef veritabanının adını ayarlayın.|
|**output_schema_name** |nvarchar(max)| Hedef şema adı. Varsayılan olarak belirtilmezse, dbo.|
|**output_table_name**| nvarchar(max)|  Sorgu sonuçları ayarlayın Sonuçların depolanacağı tablonun adı. Tablo, zaten yoksa ayarlamak sonuçları şemasını temel alarak otomatik olarak oluşturulur. Şema sonuç kümesi şeması ile eşleşmesi gerekir.|
|**max_parallelism**|   Int|    Veritabanları işi adımı aynı anda üzerinde çalıştırılacak esnek havuz başına maksimum sayısı. Varsayılan sınır anlamı NULL ' dır. |


### <a name="jobstepversions-view"></a>jobstep_versions görünümü

[işleri]. [jobstep_versions]

Tüm adımları her işi tüm sürümlerini gösterir. Şema aynıdır [iş adımları](#jobsteps-view).

### <a name="targetgroups-view"></a>target_groups görünümü

[işleri]. [target_groups]

Tüm hedef grupları listeler.

|Sütun adı|Veri türü| Açıklama|
|-----|-----|-----|
|**target_group_name**| Nvarchar(128)   |Hedef grup, veritabanlarının bir koleksiyon adı. 
|**target_group_id**    |benzersiz tanımlayıcı   |Hedef grubun benzersiz kimliği.

### <a name="targetgroupsmembers-view"></a>target_groups_members görünümü

[işleri]. [target_groups_members]

Tüm hedef grupları tüm üyeleri gösterir.

|Sütun adı|Veri türü| Açıklama|
|-----|-----|-----|
|**target_group_name**  |nvarchar (128|Hedef grup, veritabanlarının bir koleksiyon adı. |
|**target_group_id**    |benzersiz tanımlayıcı   |Hedef grubun benzersiz kimliği.|
|**membership_type**    |Int|   Hedef grup üyesi ekleneceğini veya hedef grup olmadığını belirtir. 'Include' veya 'Dışla' target_group_name için geçerli değerler.|
|**target_type**    |Nvarchar(128)| Hedef veritabanı veya sunucu, esnek havuzdaki tüm veritabanları veya bir veritabanında tüm veritabanları da dahil olmak üzere veritabanları koleksiyonunu türü. 'SqlServer', 'SqlElasticPool', 'SqlDatabase' veya 'SqlShardMap' target_type için geçerli değerler.|
|**target_id**  |benzersiz tanımlayıcı|  Hedef grup üyesi benzersiz kimliği.|
|**refresh_credential_name**    |Nvarchar(128)  |Veritabanının adı hedef grubu üyesine bağlanmak için kullanılan kimlik bilgilerini kapsamlı.|
|**subscription_id**    |benzersiz tanımlayıcı|  Abonelik benzersiz kimliği.|
|**resource_group_name**    |Nvarchar(128)| Hedef grup üyesi bulunduğu kaynak grubunun adı.|
|**sunucu_adı**    |Nvarchar(128)  |Hedef grupta bulunan mantıksal sunucunun adıdır. Yalnızca target_type 'SqlServer' kullanılıyorsa belirtilebilir. |
|**database_name**  |Nvarchar(128)  |Hedef grupta bulunan veritabanının adı. Yalnızca target_type 'SqlDatabase' olduğunda belirtildi.|
|**elastic_pool_name**  |Nvarchar(128)| Hedef grupta bulunan esnek havuz adı. Yalnızca target_type 'SqlElasticPool' olduğunda belirtildi.|
|**shard_map_name** |Nvarchar(128)| Hedef grupta bulunan parça eşlemesinin adı. Yalnızca target_type 'SqlShardMap' olduğunda belirtildi.|


## <a name="resources"></a>Kaynaklar

 - ![Konu bağlantı simgesi](https://docs.microsoft.com/sql/database-engine/configure-windows/media/topic-link.gif "konu bağlantı simgesi") [Transact-SQL söz dizimi kuralları](/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  


## <a name="next-steps"></a>Sonraki adımlar

- [Esnek PowerShell kullanarak işleri oluşturmak ve yönetmek](elastic-jobs-powershell.md)
- [Yetkilendirme ve izinleri SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/authorization-and-permissions-in-sql-server)
  