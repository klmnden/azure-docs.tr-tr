---
title: Oluşturma ve Azure SQL elastik veritabanı işleri Transact-SQL (T-SQL) kullanarak yönetme | Microsoft Docs
description: Komut dosyaları çok sayıda veritabanı arasında Transact-SQL (T-SQL) kullanarak elastik veritabanı İş Aracısı ile çalışır.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
ms.author: jaredmoo
author: jaredmoo
ms.reviewer: sstein
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 59e0e4cf82af9851dacf3ec030575ed392571331
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59523775"
---
# <a name="use-transact-sql-t-sql-to-create-and-manage-elastic-database-jobs"></a>Elastik veritabanı işleri oluşturmak ve yönetmek için Transact-SQL (T-SQL) kullanın

Bu makalede T-SQL kullanarak elastik işlerle çalışmaya başlamak için birçok örnek senaryolar sağlar.

Örneklerde [saklı yordamlar](#job-stored-procedures) ve [görünümleri](#job-views) bulunan [ *iş veritabanı*](sql-database-job-automation-overview.md#job-database).

Transact-SQL (T-SQL) oluşturmak, yapılandırmak, yürütme ve işlerini yönetmek için kullanılır. Elastik İş Aracısı oluşturma desteklenmiyor T-SQL, önce oluşturmanız gerekir, böylece bir *elastik İş Aracısı* , portalı kullanarak veya [PowerShell](elastic-jobs-powershell.md#create-the-elastic-job-agent).


## <a name="create-a-credential-for-job-execution"></a>İş yürütme için bir kimlik bilgisi oluşturma

Betiğin yürütülmesi için hedef veritabanlarına bağlanmak için kimlik bilgisi kullanılır. Kimlik bilgisi başarıyla betiği yürütmek için uygun izinleri, hedef grubu tarafından belirtilen veritabanları üzerinde olması gerekir. Bir sunucu ve/veya havuzu hedef grup üyesi kullanırken, yüksek oranda server ve/veya havuz, iş yürütme zamanında genişletilmesi önce kimlik bilgilerini yenilemek kullanılacak ana kimlik bilgisini oluşturmak için önerilir. Veritabanı kapsamlı kimlik bilgileri, İş Aracısı veritabanı üzerinde oluşturulur. Aynı kimlik bilgisi için kullanılan *bir oturum açma oluşturma* ve *bir kullanıcı oturum açma veritabanı izinleri vermek için oturumu açma oluşturun* hedef veritabanları üzerinde.


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

## <a name="create-a-target-group-servers"></a>Bir hedef grup (Sunucuları) oluşturma

Aşağıdaki örnek, bir sunucu bir işin tüm veritabanlarında yürütün gösterilmektedir.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:


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


## <a name="exclude-an-individual-database"></a>Tek bir veritabanını hariç tut

Aşağıdaki örnekte adlı veritabanı dışında bir SQL veritabanı sunucusu bir işi tüm veritabanlarında yürütün gösterilmiştir *MappingDB*.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC [jobs].sp_add_target_group N'ServerGroup'
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

--Exclude a database target member from the server target group
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


## <a name="create-a-target-group-pools"></a>Bir hedef grup (havuzları) oluşturma

Aşağıdaki örnek, bir veya daha fazla esnek havuzlar içindeki tüm veritabanlarına hedef gösterilmektedir.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

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


## <a name="deploy-new-schema-to-many-databases"></a>Çok sayıda veritabanı için yeni şemayı dağıtma

Aşağıdaki örnek, tüm veritabanları için yeni şemayı dağıtma gösterilmektedir.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:


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


## <a name="data-collection-using-built-in-parameters"></a>Yerleşik parametreleri kullanarak veri toplama

Birçok veri toplama senaryolarda, bazı iş sonuçlarını işlem sonrası yardımcı olmak için bu komut dosyası değişkenleri dahil etmek yararlı olabilir.

- $(job_name)
- $(job_id)
- $(job_version)
- $(step_id)
- $(step_name)
- $(job_execution_id)
- $(job_execution_create_time)
- $(target_group_name)

Örneğin, aynı iş yürütme birlikte gelen tüm sonuçları gruplandırmak için kullanmak *$(job_execution_id)* aşağıdaki komutta gösterildiği gibi:


```sql
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());'
```


## <a name="monitor-database-performance"></a>Veritabanı performansını izleme

Aşağıdaki örnek, birden çok veritabanlarından performans verilerini toplamak için yeni bir iş oluşturur.

Varsayılan olarak, döndürülen sonuçlarda depolamak için bir tablo oluşturmak için İş Aracısı görünecektir. Sonuç olarak çıktı kimlik bilgileri kullanılan kimlik bilgileri ile ilişkili oturum açma, bunu gerçekleştirmek için yeterli izinlere sahip gerekecektir. Daha sonra el ile tablo önceden oluşturmak istiyorsanız aşağıdaki özelliklere sahip olması gerekir:
1. Sütun adının doğru ve veri türleri için sonuç kümesi.
2. Benzersiz tanımlayıcı veri türünde internal_execution_id ek sütun.
3. Adlı kümelenmemiş bir dizin `IX_<TableName>_Internal_Execution_ID` internal_execution_id sütunu.

Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutları çalıştırın:

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
@output_credential_name='myjobcred',
@output_server_name='server1.database.windows.net',
@output_database_name='<resultsdb>',
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
@output_server_name='server1.database.windows.net',
@output_database_name='resultsdb',
@output_table_name='resutlstable'
```


## <a name="view-job-definitions"></a>İş Tanımları Görüntüle

Aşağıdaki örnek, geçerli iş tanımlarını görüntüleme gösterir.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

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

Aşağıdaki örnek, bir işi hemen başlatmak gösterilmektedir.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

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

Aşağıdaki örnek, gelecekteki yürütme için bir iş zamanlama gösterilmektedir.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_update_job
@job_name='ResultsJob',
@enabled=1,
@schedule_interval_type='Minutes',
@schedule_interval_count=15
```

## <a name="monitor-job-execution-status"></a>İş yürütme durumunu izleme

Aşağıdaki örnek, tüm işler için yürütme durum bilgilerini görüntülemek gösterilmektedir.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

--View top-level execution status for the job named 'ResultsPoolJob'
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' and step_id IS NULL
ORDER BY start_time DESC

--View all top-level execution status for all jobs
SELECT * FROM jobs.job_executions WHERE step_id IS NULL
ORDER BY start_time DESC

--View all execution statuses for job named 'ResultsPoolsJob'
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' 
ORDER BY start_time DESC

-- View all active executions
SELECT * FROM jobs.job_executions 
WHERE is_active = 1
ORDER BY start_time DESC
```


## <a name="cancel-a-job"></a>Bir işi iptal et

Aşağıdaki örnek, bir işi iptal etmek gösterilmektedir.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

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
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Delete history of a specific job’s executions older than the specified date
EXEC jobs.sp_purge_jobhistory @job_name='ResultPoolsJob', @oldest_date='2016-07-01 00:00:00'

--Note: job history is automatically deleted if it is >45 days old
```

## <a name="delete-a-job-and-all-its-job-history"></a>Bir iş ve buna ait iş Geçmişi Sil

Aşağıdaki örnek, bir işi silmek gösterilir ve ilgili tüm iş geçmişi.  
Bağlanma [ *iş veritabanı* ](sql-database-job-automation-overview.md#job-database) ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_delete_job @job_name='ResultsPoolsJob'

--Note: job history is automatically deleted if it is >45 days old
```




## <a name="job-stored-procedures"></a>İş saklı yordamları

Aşağıdaki saklı yordamlara bulunan [işleri veritabanı](sql-database-job-automation-overview.md#job-database).



|Saklı yordam  |Açıklama  |
|---------|---------|
|[sp_add_job](#sp_add_job)     |     Yeni Proje ekler.    |
|[sp_update_job](#sp_update_job)    |      Var olan bir işi güncelleştirir.   |
|[sp_delete_job](#sp_delete_job)     |      Var olan bir işi siler.   |
|[sp_add_jobstep](#sp_add_jobstep)    |    Bir adım, bir projeye ekler.     |
|[sp_update_jobstep](#sp_update_jobstep)     |     Bir iş adımı güncelleştirir.    |
|[sp_delete_jobstep](#sp_delete_jobstep)     |     Bir iş adımı siler.    |
|[sp_start_job](#sp_start_job)    |  Bir işi yürütmeden başlatır.       |
|[sp_stop_job](#sp_stop_job)     |     İş yürütmeyi durdurur.   |
|[sp_add_target_group](#sp_add_target_group)    |     Bir hedef grubu ekler.    |
|[sp_delete_target_group](#sp_delete_target_group)     |    Hedef grubunu siler.     |
|[sp_add_target_group_member](#sp_add_target_group_member)     |    Bir veritabanı veya veritabanı grubu için bir hedef grubu ekler.     |
|[sp_delete_target_group_member](#sp_delete_target_group_member)     |     Hedef grup üyesi hedef gruptan kaldırır.    |
|[sp_purge_jobhistory](#sp_purge_jobhistory)    |    Bir iş geçmişi kayıtları kaldırır.     |





### <a name="spaddjob"></a>sp_add_job

Yeni Proje ekler. 
  
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

[  **\@job_name =** ] 'job_name'  
İş adı. Adı benzersiz olmalıdır ve yüzde (%) içeremez karakter. job_name varsayılansız bir nvarchar(128) ' dir.

[  **\@açıklama =** ] 'description'  
Proje açıklaması. Varsayılan null bir nvarchar(512) açıklamasıdır. Açıklama atlanırsa, boş bir dize kullanılır.

[  **\@etkin =** ] etkin  
İş zamanlamasını etkinleştirilip etkinleştirilmediği. Etkin, varsayılan olarak 0 (devre dışı) ile bit. 0 ise, iş etkin değil ve kendi zamanlamasına göre çalışmaz; Ancak, bunu el ile çalıştırılabilir. 1 ise, iş, zamanlamaya göre çalışır ve el ile de çalıştırılabilir.

[ **\@schedule_interval_type =**] schedule_interval_type  
Yürütülecek iş olduğunda değeri gösterir. schedule_interval_type nvarchar(50), bir kez, varsayılan ve aşağıdaki değerlerden biri olabilir:
- 'Bir kez'
- 'Minutes'
- 'Hours'
- 'Days'
- 'Hafta'
- 'Months'

[  **\@schedule_interval_count =** ] schedule_interval_count  
Her işin yürütülmesi arasında gerçekleşecek şekilde schedule_interval_count nokta sayısı. schedule_interval_count int, varsayılan değer olan 1 ' dir. Değer 1'e eşit veya daha büyük olmalıdır.

[  **\@schedule_start_time =** ] schedule_start_time  
Tarih üzerinde hangi iş yürütme başlayabilirsiniz. schedule_start_time DATETIME2, 0001-01-01 00:00:00.0000000 varsayılan değer ' dir.

[  **\@schedule_end_time =** ] schedule_end_time  
Tarih üzerinde hangi iş yürütme durdurabilirsiniz. schedule_end_time, DATETIME2, 9999-12-31 ile varsayılan 11:59:59.0000000. 

[  **\@job_id =** ] job_id çıkış  
Başarıyla oluşturulursa, işe atanan iş kimlik numarası. job_id, türü benzersiz tanımlayıcı bir çıkış değişkenidir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri

(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı sp_add_job çalıştırılması gerekir.
Bir proje eklemek için sp_add_job yürütüldükten sonra sp_add_jobstep iş için etkinlikleri gerçekleştiren adımları eklemek için kullanılabilir. İşin ilk sürüm numarası, ilk adım eklendiğinde 1 artırılır 0 ' dır.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:

- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.

### <a name="spupdatejob"></a>sp_update_job

Var olan bir işi güncelleştirir.

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
[  **\@job_name =** ] 'job_name'  
Güncelleştirilecek bir proje adı. job_name nvarchar(128) ' dir.

[  **\@new_name =** ] 'new_name'  
İş yeni adı. new_name nvarchar(128) ' dir.

[  **\@açıklama =** ] 'description'  
Proje açıklaması. nvarchar(512) açıklamasıdır.

[  **\@etkin =** ] etkin  
İş zamanlamasını (1) etkin olup olmadığını belirtir ya da (0) etkin değil. Etkin bit.

[ **\@schedule_interval_type=** ] schedule_interval_type  
Yürütülecek iş olduğunda değeri gösterir. schedule_interval_type nvarchar(50) ve aşağıdaki değerlerden biri olabilir:

- 'Bir kez'
- 'Minutes'
- 'Hours'
- 'Days'
- 'Hafta'
- 'Months'

[ **\@schedule_interval_count=** ] schedule_interval_count  
Her işin yürütülmesi arasında gerçekleşecek şekilde schedule_interval_count nokta sayısı. schedule_interval_count int, varsayılan değer olan 1 ' dir. Değer 1'e eşit veya daha büyük olmalıdır.

[  **\@schedule_start_time =** ] schedule_start_time  
Tarih üzerinde hangi iş yürütme başlayabilirsiniz. schedule_start_time DATETIME2, 0001-01-01 00:00:00.0000000 varsayılan değer ' dir.

[  **\@schedule_end_time =** ] schedule_end_time  
Tarih üzerinde hangi iş yürütme durdurabilirsiniz. schedule_end_time, DATETIME2, 9999-12-31 ile varsayılan 11:59:59.0000000. 

#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Bir proje eklemek için sp_add_job yürütüldükten sonra sp_add_jobstep iş için etkinlikleri gerçekleştiren adımları eklemek için kullanılabilir. İşin ilk sürüm numarası, ilk adım eklendiğinde 1 artırılır 0 ' dır.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.



### <a name="spdeletejob"></a>sp_delete_job

Var olan bir işi siler.

#### <a name="syntax"></a>Sözdizimi

```sql
[jobs].sp_delete_job [ @job_name = ] 'job_name'
    [ , [ @force = ] force ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **\@job_name =** ] 'job_name'  
Silinecek iş adı. job_name nvarchar(128) ' dir.

[  **\@zorla =** ] zorla  
İşi devam eden tüm yürütmeleri yoksa silme ve tüm iş yürütme sayısı (0) sürmekte olan tüm devam eden yürütmeler (1) veya başarısız iptal belirtir. zorla bit.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
İş geçmişi, bir işi silindiğinde otomatik olarak silinir.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.



### <a name="spaddjobstep"></a>sp_add_jobstep

Bir adım, bir projeye ekler.

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

[  **\@job_name =** ] 'job_name'  
Adım ekleme yapılacak işin adı. job_name nvarchar(128) ' dir.

[ **\@step_id =** ] step_id  
İş adımı dizisi kimlik numarası. Adım kimlik numaraları 1'den başlar ve boşluk artırın. Var olan bir adım bu kimliği zaten varsa, bu yeni bir adım dizisi olarak eklenebilir böylece ardından adım ve aşağıdaki adımların tümünü kimliklerine sahip olacağını artırılır. Belirtilmezse, step_id otomatik olarak adımları sırayla son atanır. step_id bir tamsayı.

[  **\@step_name =** ] step_name  
Adım adı. , (Kolaylık) 'JobStep' varsayılan adını taşıyan bir işi ilk adımı hariç belirtilmelidir. step_name nvarchar(128) ' dir.

[ **\@command_type =** ] 'command_type'  
Bu jobstep tarafından yürütülen komut türü. command_type nvarchar(50), TSql değerini, yani, varsayılan değeri olan @command_type bir T-SQL betiği parametredir.

Belirtilmişse, değerin TSql olması gerekir.

[ **\@command_source =** ] 'command_source'  
Komut depolandığı konumun türü. command_source olduğundan, satır içi değerini, yani, varsayılan bir değerle nvarchar(50) @command_source parametresi, komutun metin.

Belirtilmişse, değerin satır içi olması gerekir.

[  **\@komut =** ] 'command'  
Komut geçerli T-SQL komut dosyası olması gerekir ve bu iş adımı tarafından yürütülür. nvarchar(max), varsayılan değeri NULL ile bir komuttur.

[  **\@credential_name =** ] 'credential_name'  
Veritabanının adı, bu adım çalıştırıldığında her hedef grubu içindeki hedef veritabanlarına bağlanmak için kullanılan bu iş kontrol veritabanına depolanmış kimlik kapsamı. credential_name nvarchar(128) ' dir.

[  **\@target_group_name =** ] 'hedef grup_adı'  
İçeren iş adımını üzerinde yürütülen hedef veritabanlarına hedef grubun adı. target_group_name nvarchar(128) ' dir.

[  **\@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
İlk yeniden deneme çalışmadan önce iş adımı ilk yürütme denemede başarısız olursa gecikmeyi. initial_retry_interval_seconds int, varsayılan değer olan 1 ' dir.

[ **\@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
Yeniden deneme girişimleri arasındaki en büyük gecikme. Yeniden denemeler arasındaki gecikme bu değerden daha büyük geçerseniz, bunun yerine bu değere tavan sabitlenir. maximum_retry_interval_seconds int, varsayılan değer 120 ' dir.

[  **\@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
Birden çok iş yürütme adım durumunda yeniden deneme gecikmesi uygulanacak çarpan denemesi başarısız. Örneğin, ilk yeniden deneme 5 saniyede bir gecikme sahipti ve geri alma çarpan 2.0 ise, ardından ikinci bir yeniden deneme 10 saniyede bir gecikme olur ve üçüncü yeniden 20 saniye gecikme gerekir. retry_interval_backoff_multiplier 2.0 varsayılan değeri ile gerçek.

[ **\@retry_attempts =** ] retry_attempts  
Yürütme ilk denemesi başarısız olursa yeniden deneme sayısı. Olacaktır 1 ilk retry_attempts değeri 10, örneğin, girişimini ve 10 11 denemelerinin toplam vererek, yeniden deneme sayısı. Son yeniden deneme girişimi başarısız olursa, iş yürütme başarısız bir yaşam döngüsü ile sona erer. retry_attempts int, varsayılan 10 değerini taşıyan ' dir.

[  **\@step_timeout_seconds =** ] step_timeout_seconds  
En fazla adım yürütmek izin verilen süre. Bu süre aşılırsa, iş yürütme süresi sona erdi bir yaşam döngüsü ile sona erer. step_timeout_seconds int, değer 43.200 saniye (12 saat) varsayılan değere sahip olur.

[  **\@output_type =** ] 'output_type'  
Aksi durumda, komutun ilk sonuç kümesi hedef türü için null yazılır. output_type nvarchar(50), varsayılan değer null olur.

Belirtilmişse, değerin SqlDatabase olması gerekir.

[  **\@output_credential_name =** ] 'output_credential_name'  
Boş değilse, veritabanının adı kapsamlı çıkış hedef veritabanına bağlanmak için kullanılan kimlik bilgileri kullanılır. SQL veritabanı output_type eşitse belirtilmelidir. output_credential_name nvarchar(128), varsayılan değeri null olur.

[  **\@output_subscription_id =** ] 'output_subscription_id'  
Açıklama gereklidir.

[  **\@output_resource_group_name =** ] 'output_resource_group_name'  
Açıklama gereklidir.

[  **\@output_server_name =** ] 'output_server_name'  
Aksi durumda null, çıkış hedef veritabanını içeren sunucusunun tam DNS adı. SQL veritabanı output_type eşitse belirtilmelidir. output_server_name nvarchar(256), varsayılan değer null olur.

[  **\@output_database_name =** ] 'output_database_name'  
Aksi durumda null, çıktı hedef tablosu içeren veritabanının adı. SQL veritabanı output_type eşitse belirtilmelidir. output_database_name nvarchar(128), varsayılan değer null olur.

[  **\@output_schema_name =** ] 'output_schema_name'  
Aksi durumda null, çıktı hedef tablosu içeren SQL şemasının adı. SQL veritabanı output_type eşit ise varsayılan değer dbo ' dir. output_schema_name nvarchar(128) ' dir.

[  **\@output_table_name =** ] 'output_table_name'  
Aksi durumda null, komutun ilk sonuç kümesi tablonun adını yazılır. Tablo zaten yoksa, sonuç kümesi döndüren şema göre oluşturulur. SQL veritabanı output_type eşitse belirtilmelidir. output_table_name nvarchar(128), varsayılan değeri null olur.

[  **\@job_version =** ] job_version çıkış  
Yeni proje sürüm numarasını atanacak çıkış parametresi. job_version tamsayı.

[  **\@max_parallelism =** ] max_parallelism çıkış  
Paralellik elastik havuz başına en fazla düzeyi. Küme ve iş adımı yalnızca çok sayıda veritabanı elastik havuz başına en çok, üzerinde çalıştırmak için kısıtlı olacaksa. Bu, ya da doğrudan hedef gruba dahil veya hedef grupta yer alan bir sunucu içindeki her esnek havuz için geçerlidir. max_parallelism tamsayı.


#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Sp_add_jobstep başarılı olduğunda, işin geçerli sürüm numarası artırılır. Yeni sürüm, işin yürütüldüğünde, sonraki açışınızda kullanılacaktır. İş yürütülmekte olduğundan, bu yürütme yeni adım içermez.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:  

- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.



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
[  **\@job_name =** ] 'job_name'  
Adım ait olduğu proje adı. job_name nvarchar(128) ' dir.

[ **\@step_id =** ] step_id  
Değiştirilecek iş adımı kimlik numarası. Step_id ya da step_name belirtilmesi gerekir. step_id bir tamsayı.

[  **\@step_name =** ] 'step_name'  
Değiştirilecek adım adı. Step_id ya da step_name belirtilmesi gerekir. step_name nvarchar(128) ' dir.

[ **\@new_id =** ] new_id  
İş adımı için yeni sıra kimlik numarası. Adım kimlik numaraları 1'den başlar ve boşluk artırın. Bir adım sıralanır, diğer adımları otomatik olarak numaralandırılır.

[  **\@new_name =** ] 'new_name'  
Adım yeni adı. new_name nvarchar(128) ' dir.

[ **\@command_type =** ] 'command_type'  
Bu jobstep tarafından yürütülen komut türü. command_type nvarchar(50), TSql değerini, yani, varsayılan değeri olan @command_type bir T-SQL betiği parametredir.

Belirtilmişse, değerin TSql olması gerekir.

[ **\@command_source =** ] 'command_source'  
Komut depolandığı konumun türü. command_source olduğundan, satır içi değerini, yani, varsayılan bir değerle nvarchar(50) @command_source parametresi, komutun metin.

Belirtilmişse, değerin satır içi olması gerekir.

[  **\@komut =** ] 'command'  
Komut geçerli T-SQL komut dosyası olması gerekir ve bu iş adımı tarafından yürütülür. nvarchar(max), varsayılan değeri NULL ile bir komuttur.

[  **\@credential_name =** ] 'credential_name'  
Veritabanının adı, bu adım çalıştırıldığında her hedef grubu içindeki hedef veritabanlarına bağlanmak için kullanılan bu iş kontrol veritabanına depolanmış kimlik kapsamı. credential_name nvarchar(128) ' dir.

[  **\@target_group_name =** ] 'hedef grup_adı'  
İçeren iş adımını üzerinde yürütülen hedef veritabanlarına hedef grubun adı. target_group_name nvarchar(128) ' dir.

[  **\@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
İlk yeniden deneme çalışmadan önce iş adımı ilk yürütme denemede başarısız olursa gecikmeyi. initial_retry_interval_seconds int, varsayılan değer olan 1 ' dir.

[ **\@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
Yeniden deneme girişimleri arasındaki en büyük gecikme. Yeniden denemeler arasındaki gecikme bu değerden daha büyük geçerseniz, bunun yerine bu değere tavan sabitlenir. maximum_retry_interval_seconds int, varsayılan değer 120 ' dir.

[  **\@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
Birden çok iş yürütme adım durumunda yeniden deneme gecikmesi uygulanacak çarpan denemesi başarısız. Örneğin, ilk yeniden deneme 5 saniyede bir gecikme sahipti ve geri alma çarpan 2.0 ise, ardından ikinci bir yeniden deneme 10 saniyede bir gecikme olur ve üçüncü yeniden 20 saniye gecikme gerekir. retry_interval_backoff_multiplier 2.0 varsayılan değeri ile gerçek.

[ **\@retry_attempts =** ] retry_attempts  
Yürütme ilk denemesi başarısız olursa yeniden deneme sayısı. Olacaktır 1 ilk retry_attempts değeri 10, örneğin, girişimini ve 10 11 denemelerinin toplam vererek, yeniden deneme sayısı. Son yeniden deneme girişimi başarısız olursa, iş yürütme başarısız bir yaşam döngüsü ile sona erer. retry_attempts int, varsayılan 10 değerini taşıyan ' dir.

[  **\@step_timeout_seconds =** ] step_timeout_seconds  
En fazla adım yürütmek izin verilen süre. Bu süre aşılırsa, iş yürütme süresi sona erdi bir yaşam döngüsü ile sona erer. step_timeout_seconds int, değer 43.200 saniye (12 saat) varsayılan değere sahip olur.

[  **\@output_type =** ] 'output_type'  
Aksi durumda, komutun ilk sonuç kümesi hedef türü için null yazılır. Output_type değerini tekrar NULL olarak ayarlamak için bu parametrenin değerini ayarlamak '' (boş dize). output_type nvarchar(50), varsayılan değer null olur.

Belirtilmişse, değerin SqlDatabase olması gerekir.

[  **\@output_credential_name =** ] 'output_credential_name'  
Boş değilse, veritabanının adı kapsamlı çıkış hedef veritabanına bağlanmak için kullanılan kimlik bilgileri kullanılır. SQL veritabanı output_type eşitse belirtilmelidir. Output_credential_name değerini tekrar NULL olarak ayarlamak için bu parametrenin değerini ayarlamak '' (boş dize). output_credential_name nvarchar(128), varsayılan değeri null olur.

[  **\@output_server_name =** ] 'output_server_name'  
Aksi durumda null, çıkış hedef veritabanını içeren sunucusunun tam DNS adı. SQL veritabanı output_type eşitse belirtilmelidir. Output_server_name değerini tekrar NULL olarak ayarlamak için bu parametrenin değerini ayarlamak '' (boş dize). output_server_name nvarchar(256), varsayılan değer null olur.

[  **\@output_database_name =** ] 'output_database_name'  
Aksi durumda null, çıktı hedef tablosu içeren veritabanının adı. SQL veritabanı output_type eşitse belirtilmelidir. Output_database_name değerini tekrar NULL olarak ayarlamak için bu parametrenin değerini ayarlamak '' (boş dize). output_database_name nvarchar(128), varsayılan değer null olur.

[  **\@output_schema_name =** ] 'output_schema_name'  
Aksi durumda null, çıktı hedef tablosu içeren SQL şemasının adı. SQL veritabanı output_type eşit ise varsayılan değer dbo ' dir. Output_schema_name değerini tekrar NULL olarak ayarlamak için bu parametrenin değerini ayarlamak '' (boş dize). output_schema_name nvarchar(128) ' dir.

[  **\@output_table_name =** ] 'output_table_name'  
Aksi durumda null, komutun ilk sonuç kümesi tablonun adını yazılır. Tablo zaten yoksa, sonuç kümesi döndüren şema göre oluşturulur. SQL veritabanı output_type eşitse belirtilmelidir. Output_server_name değerini tekrar NULL olarak ayarlamak için bu parametrenin değerini ayarlamak '' (boş dize). output_table_name nvarchar(128), varsayılan değeri null olur.

[  **\@job_version =** ] job_version çıkış  
Yeni proje sürüm numarasını atanacak çıkış parametresi. job_version tamsayı.

[  **\@max_parallelism =** ] max_parallelism çıkış  
Paralellik elastik havuz başına en fazla düzeyi. Küme ve iş adımı yalnızca çok sayıda veritabanı elastik havuz başına en çok, üzerinde çalıştırmak için kısıtlı olacaksa. Bu, ya da doğrudan hedef gruba dahil veya hedef grupta yer alan bir sunucu içindeki her esnek havuz için geçerlidir. Max_parallelism değerini tekrar null olarak sıfırlamak için bu parametrenin değerini -1 olarak ayarlayın. max_parallelism tamsayı.


#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
İşin tüm devam eden yürütmeleri etkilenmez. Sp_update_jobstep başarılı olduğunda, işin sürüm numarası artırılır. Yeni sürüm, işin yürütüldüğünde, sonraki açışınızda kullanılacaktır.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:

- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordamı diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz




### <a name="spdeletejobstep"></a>sp_delete_jobstep

Bir iş adımı, bir iş kaldırır.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_delete_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **\@job_name =** ] 'job_name'  
Adım kaldırılacak iş adı. job_name varsayılansız bir nvarchar(128) ' dir.

[ **\@step_id =** ] step_id  
Silinecek kimlik numarası işi adımı. Step_id ya da step_name belirtilmesi gerekir. step_id bir tamsayı.

[  **\@step_name =** ] 'step_name'  
Silinecek adım adı. Step_id ya da step_name belirtilmesi gerekir. step_name nvarchar(128) ' dir.

[  **\@job_version =** ] job_version çıkış  
Yeni proje sürüm numarasını atanacak çıkış parametresi. job_version tamsayı.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
İşin tüm devam eden yürütmeleri etkilenmez. Sp_update_jobstep başarılı olduğunda, işin sürüm numarası artırılır. Yeni sürüm, işin yürütüldüğünde, sonraki açışınızda kullanılacaktır.

Bir iş adımları silinmiş bir iş adımı tarafından sol boşluğu doldurmak için otomatik olarak numaralandırılır.
 
#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.






### <a name="spstartjob"></a>sp_start_job

Bir işi yürütmeden başlatır.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_start_job [ @job_name = ] 'job_name'   
     [ , [ @job_execution_id = ] job_execution_id OUTPUT ]   
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **\@job_name =** ] 'job_name'  
Adım kaldırılacak iş adı. job_name varsayılansız bir nvarchar(128) ' dir.

[  **\@job_execution_id =** ] job_execution_id çıkış  
Çıkış iş yürütme'nın kimlik atanacak parametresi. job_version uniqueidentifier ' dir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Yok.
 
#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.

### <a name="spstopjob"></a>sp_stop_job

İş yürütmeyi durdurur.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_stop_job [ @job_execution_id = ] ' job_execution_id '
```


#### <a name="arguments"></a>Bağımsız Değişkenler
[  **\@job_execution_id =** ] job_execution_id  
Durdurmak için iş yürütme kimliği sayısı. Varsayılan null ile benzersiz tanımlayıcı job_execution_id olur.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Yok.
 
#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.


### <a name="spaddtargetgroup"></a>sp_add_target_group

Bir hedef grubu ekler.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_add_target_group [ @target_group_name = ] 'target_group_name'   
     [ , [ @target_group_id = ] target_group_id OUTPUT ]
```


#### <a name="arguments"></a>Bağımsız Değişkenler
[  **\@target_group_name =** ] 'target_group_name'  
Oluşturulacak hedef grubun adı. target_group_name varsayılansız bir nvarchar(128) ' dir.

[  **\@target_group_id =** ] target_group_id çıkış hedef grup başarıyla oluşturuldu, işe atanan kimlik numarası. target_group_id varsayılan NULL, türü benzersiz tanımlayıcı, bir çıkış değişkenidir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Hedef grupları veritabanları koleksiyonunu bir işi hedeflemek için kolay bir yol sağlar.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.

### <a name="spdeletetargetgroup"></a>sp_delete_target_group

Hedef grubunu siler.

#### <a name="syntax"></a>Sözdizimi


```sql
[jobs].sp_delete_target_group [ @target_group_name = ] 'target_group_name'
```


#### <a name="arguments"></a>Bağımsız Değişkenler
[  **\@target_group_name =** ] 'target_group_name'  
Silmek için hedef grubun adı. target_group_name varsayılansız bir nvarchar(128) ' dir.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Yok.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.

### <a name="spaddtargetgroupmember"></a>sp_add_target_group_member

Bir veritabanı veya veritabanı grubu için bir hedef grubu ekler.

#### <a name="syntax"></a>Sözdizimi

```sql
[jobs].sp_add_target_group_member [ @target_group_name = ] 'target_group_name'
         [ @membership_type = ] 'membership_type' ]   
        [ , [ @target_type = ] 'target_type' ]   
        [ , [ @refresh_credential_name = ] 'refresh_credential_name' ]   
        [ , [ @server_name = ] 'server_name' ]   
        [ , [ @database_name = ] 'database_name' ]   
        [ , [ @elastic_pool_name = ] 'elastic_pool_name' ]   
        [ , [ @shard_map_name = ] 'shard_map_name' ]   
        [ , [ @target_id = ] 'target_id' OUTPUT ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[  **\@target_group_name =** ] 'target_group_name'  
Üye eklenecek hedef grubun adı. target_group_name varsayılansız bir nvarchar(128) ' dir.

[ **\@membership_type =** ] 'membership_type'  
Hedef grup üyesi dahil dışlanan ya da söndürüleceğini belirler. target_group_name nvarchar(128), 'Ekle' varsayılan'ile ' dir. 'Include' veya 'Exclude' target_group_name için geçerli değerlerdir.

[  **\@target_type =** ] 'target_type'  
Hedef veritabanı veya sunucu tüm veritabanları, elastik bir havuzdaki tüm veritabanları, bir parça eşlemesi içindeki tüm veritabanlarına veya tek bir veritabanı gibi veritabanlarının koleksiyon türü. target_type varsayılansız bir nvarchar(128) ' dir. 'SqlServer', 'SqlElasticPool', 'Temel' veya 'SqlShardMap' target_type için geçerli değerlerdir. 

[  **\@refresh_credential_name =** ] 'refresh_credential_name'  
SQL veritabanı sunucu adı. refresh_credential_name varsayılansız bir nvarchar(128) ' dir.

[  **\@sunucu_adı =** ] 'sunucu_adı'  
Belirtilen hedef gruba eklenmelidir SQL veritabanı sunucu adı. target_type 'SqlServer' olduğunda sunucu_adı belirtilmelidir. SERVER_NAME varsayılansız bir nvarchar(128) ' dir.

[  **\@database_name =** ] 'database_name'  
Belirtilen hedef gruba eklenmelidir veritabanının adı. target_type 'Temel' olduğunda database_name belirtilmelidir. database_name varsayılansız bir nvarchar(128) ' dir.

[  **\@elastic_pool_name =** ] 'elastic_pool_name'  
Belirtilen hedef gruba eklenmelidir elastik havuzunun adı. target_type 'SqlElasticPool' olduğunda elastic_pool_name belirtilmelidir. elastic_pool_name varsayılansız bir nvarchar(128) ' dir.

[ **\@shard_map_name =** ] 'shard_map_name'  
Belirtilen hedef gruba eklenmelidir parça eşlemesi havuzunun adı. target_type 'SqlSqlShardMap' olduğunda elastic_pool_name belirtilmelidir. shard_map_name varsayılansız bir nvarchar(128) ' dir.

[  **\@target_id =** ] target_group_id çıkış  
Hedef kimlik numarası oluşturduysanız hedef grubu üyesine atanmış hedef gruba eklenir. target_id varsayılan NULL, türü benzersiz tanımlayıcı, bir çıkış değişkenidir.
Dönüş kodu değerler 0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Şirket içinde SQL veritabanı sunucusu veya elastik bir havuzdaki tüm tek veritabanları, SQL veritabanı sunucusu, yürütme sırasında bir iş yürütür veya elastik havuz hedef gruba dahil edildi.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.

#### <a name="examples"></a>Örnekler
Aşağıdaki örnek, tüm veritabanlarına Londra ve NewYork sunucular sunucuları müşteri bilgilerini koruma gruba ekler. İş Aracısı, bu büyük/küçük harf ElasticJobs oluştururken belirttiğiniz işleri veritabanına bağlanmalısınız.

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
        [ , [ @target_id = ] 'target_id']
```



Bağımsız değişkenler [ @target_group_name =] 'target_group_name'  
Hedef grup üyesini kaldırmak için hedef grubun adı. target_group_name varsayılansız bir nvarchar(128) ' dir.

[ @target_id =] target_id  
 Hedef kimlik numarası kaldırılması hedef grubu üyesine atanmış. Varsayılan olarak NULL ile benzersiz tanımlayıcı target_id olur.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Hedef grupları veritabanları koleksiyonunu bir işi hedeflemek için kolay bir yol sağlar.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.

#### <a name="examples"></a>Örnekler
Aşağıdaki örnek Londra sunucuyu sunucuları müşteri bilgilerini koruma grubundan kaldırır. İş Aracısı, bu büyük/küçük harf ElasticJobs oluştururken belirttiğiniz işleri veritabanına bağlanmalısınız.

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
[  **\@job_name =** ] 'job_name'  
İş Geçmişi kayıtları silmek istediğiniz adı. job_name nvarchar(128), varsayılan değer null olur. Job_id ya da job_name belirtilmesi gerekir, ancak her ikisini birden belirtilemez.

[ **\@job_id =** ] job_id  
 İş tanımı Silinecek kayıtlar için iş sayısı. job_id uniqueidentifier, varsayılan değer null olur. Job_id ya da job_name belirtilmesi gerekir, ancak her ikisini birden belirtilemez.

[ **\@oldest_date =** ] oldest_date  
 Geçmişte saklanacak eski kayıt. oldest_date DATETIME2, bir null ile varsayılandır. Oldest_date belirtildiğinde sp_purge_jobhistory yalnızca belirtilen değerden daha eski olan kayıtları kaldırır.

#### <a name="return-code-values"></a>Dönüş kodu değerleri
(başarılı) 0 veya 1 (hata) Açıklamalar hedef grupları veritabanları koleksiyonunu bir işi hedeflemek için kolay bir yol sağlar.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu saklı yordamı yürütebilir. Bunlar, yalnızca işlerini izleme kullanabilmek için bir kullanıcı kısıtlamak, aşağıdaki İş Aracısı oluştururken belirttiğiniz İş Aracısı veritabanı veritabanı rolünün bir parçası olarak kullanıcı verebilirsiniz:
- jobs_reader

Bu rollerden izinler hakkında daha fazla ayrıntı için bu belgedeki izni bölümüne bakın. Yalnızca sysadmin üyeleri bu saklı yordam, diğer kullanıcılara ait işlerin öznitelikleri düzenlemek için kullanabilirsiniz.

#### <a name="examples"></a>Örnekler
Aşağıdaki örnek, tüm veritabanlarına Londra ve NewYork sunucular sunucuları müşteri bilgilerini koruma gruba ekler. İş Aracısı, bu büyük/küçük harf ElasticJobs oluştururken belirttiğiniz işleri veritabanına bağlanmalısınız.

```sql
--Connect to the jobs database specified when creating the job agent

EXEC sp_delete_target_group_member   
    @target_group_name = N'Servers Maintaining Customer Information',  
    @server_name = N'London.database.windows.net';  
GO
```


## <a name="job-views"></a>İş görünümleri

Aşağıdaki görünümleri kullanılabilir [işleri veritabanı](sql-database-job-automation-overview.md#job-database).


|Görünüm  |Açıklama  |
|---------|---------|
|[jobs_executions](#jobs_executions-view)     |  İş yürütme geçmişini gösterir.      |
|[İşleri](#jobs-view)     |   Tüm işleri gösterir.      |
|[job_versions](#job_versions-view)     |   Tüm iş sürümlerini gösterir.      |
|[sp_reassign_proxy](#jobsteps-view)     |     Tüm adımları her projenin geçerli sürümünde gösterir.    |
|[jobstep_versions](#jobstep_versions-view)     |     Tüm adımları her bir iş tüm sürümlerini gösterir.    |
|[target_groups](#target_groups-view)     |      Tüm hedef gruplarını gösterir.   |
|[target_group_members](#target_groups_members-view)     |   Tüm hedef grupların tüm üyeleri gösterir.      |


### <a name="jobsexecutions-view"></a>jobs_executions görüntüle

[iş]. [jobs_executions]

İş yürütme geçmişini gösterir.


|Sütun adı|   Veri türü   |Açıklama|
|---------|---------|---------|
|**job_execution_id**   |benzersiz tanımlayıcı|  İş yürütme örneğini benzersiz kimliği.
|**job_name**   |nvarchar(128)  |İşin adı.
|**job_id** |benzersiz tanımlayıcı|  Projenin benzersiz kimliği.
|**job_version**    |int    |(İş değiştirilir her zaman otomatik olarak güncelleştirilir) iş sürümü.
|**step_id**    |int|   Adım için (Bu işlem için) benzersiz tanımlayıcı. NULL, bu üst iş yürütme olduğunu gösterir.
|**is_active**| Bit |Bilgi etkin veya devre dışı olup olmadığını belirtir. 1 etkin işleri gösterir ve 0 etkin olduğunu gösterir.
|**yaşam döngüsü**| nvarchar(50)|İşinin durumunu belirten değer: 'Oluşturulan', 'Sürüyor', 'Başarısız', 'Başarılı', 'Atlanan', 'SucceededWithSkipped'|
|**create_time**|   datetime2(7)|   Tarih ve saat iş oluşturuldu.
|**start_time** |datetime2(7)|  Tarih ve saat iş yürütme başlatıldı. İşi henüz yürütülmedi yoksa NULL.
|**end_time**|  datetime2(7)    |Tarih ve saat iş yürütme işlemi tamamlandı. İşi henüz yürütülmedi veya henüz yoksa NULL henüz yürütme tamamlandı.
|**current_attempts**   |int    |Adım yapılan yeniden deneme sayısı. Üst iş 0 olur, büyük yürütme ilkesini temel alarak veya alt iş yürütme sayısı 1 olacaktır.
|**current_attempt_start_time** |datetime2(7)|  Tarih ve saat iş yürütme başlatıldı. NULL, bu üst iş yürütme olduğunu gösterir.
|**last_message**   |nvarchar(max)| İş ya da adım geçmişi iletisi. 
|**target_type**|   nvarchar(128)   |Hedef veritabanı veya tüm veritabanları bir sunucu, bir elastik havuzdaki tüm veritabanları veya veritabanı gibi veritabanlarının koleksiyon türü. 'SqlServer', 'SqlElasticPool' veya 'Temel' target_type için geçerli değerlerdir. NULL, bu üst iş yürütme olduğunu gösterir.
|**target_id**  |benzersiz tanımlayıcı|  Hedef grup üyesi benzersiz kimliği.  NULL, bu üst iş yürütme olduğunu gösterir.
|**target_group_name**  |nvarchar(128)  |Hedef grubun adı. NULL, bu üst iş yürütme olduğunu gösterir.
|**target_server_name**|    nvarchar(256)|  Hedef grup içinde bulunan SQL veritabanı sunucusunun adı. Yalnızca target_type 'SqlServer' belirtilmelidir. NULL, bu üst iş yürütme olduğunu gösterir.
|**target_database_name**   |nvarchar(128)| Hedef grup içinde bulunan veritabanının adı. Target_type 'Temel' olduğunda yalnızca belirtilen. NULL, bu üst iş yürütme olduğunu gösterir.


### <a name="jobs-view"></a>işleri görüntüle

[iş]. [işleri]

Tüm işleri gösterir.

|Sütun adı|   Veri türü|  Açıklama|
|------|------|-------|
|**job_name**|  nvarchar(128)   |İşin adı.|
|**job_id**|    benzersiz tanımlayıcı    |Projenin benzersiz kimliği.|
|**job_version**    |int    |(İş değiştirilir her zaman otomatik olarak güncelleştirilir) iş sürümü.|
|**Açıklaması**    |nvarchar(512)| İş için açıklama. Etkin bit iş etkin mi yoksa devre dışı mı olduğunu belirtir. 1 etkin işleri gösterir ve 0'ı devre dışı bırakılan işler gösterir.|
|**schedule_interval_type** |nvarchar(50)   |Yürütülecek iş olduğunda belirten değer: 'Bir kez', 'Minutes', 'Hours', 'Gün', ' hafta', 'Ay'
|**schedule_interval_count**|   int|    Her işin yürütülmesi arasında gerçekleşecek şekilde schedule_interval_type nokta sayısı.|
|**schedule_start_time**    |datetime2(7)|  Tarih ve saat son başlatılan yürütme işi oluştu.|
|**schedule_end_time**| datetime2(7)|   Tarih ve saat son tamamlanan yürütme işi oluştu.|


### <a name="jobversions-view"></a>job_versions görüntüle

[iş]. [job_versions]

Tüm iş sürümlerini gösterir.

|Sütun adı|   Veri türü|  Açıklama|
|------|------|-------|
|**job_name**|  nvarchar(128)   |İşin adı.|
|**job_id**|    benzersiz tanımlayıcı    |Projenin benzersiz kimliği.|
|**job_version**    |int    |(İş değiştirilir her zaman otomatik olarak güncelleştirilir) iş sürümü.|


### <a name="jobsteps-view"></a>sp_reassign_proxy görüntüle

[iş]. [sp_reassign_proxy]

Tüm adımları her projenin geçerli sürümünde gösterir.

|Sütun adı    |Veri türü| Açıklama|
|------|------|-------|
|**job_name**   |nvarchar(128)| İşin adı.|
|**job_id** |benzersiz tanımlayıcı   |Projenin benzersiz kimliği.|
|**job_version**|   int|    (İş değiştirilir her zaman otomatik olarak güncelleştirilir) iş sürümü.|
|**step_id**    |int    |Adım için (Bu işlem için) benzersiz tanımlayıcı.|
|**step_name**  |nvarchar(128)  |Adım için (Bu işlem için) benzersiz bir ad.|
|**command_type**   |nvarchar(50)   |İş adımda yürütmek için komut türü. Değer v1 için eşit olmalıdır ve 'TSql' varsayılan değeri.|
|**command_source** |nvarchar(50)|  Komut dosyasının konumu. V1, 'Inline' varsayılandır ve yalnızca kabul edilen değer.|
|**Komutu**|   nvarchar(max)|  Esnek işler command_type aracılığıyla tarafından yürütülecek komutlar içerir.|
|**credential_name**|   nvarchar(128)   |İş yürütme için kullanılan veritabanı kapsamlı kimlik bilgisinin adı.|
|**target_group_name**| nvarchar(128)   |Hedef grubun adı.|
|**target_group_id**|   benzersiz tanımlayıcı|   Hedef grubun benzersiz kimliği.|
|**initial_retry_interval_seconds**|    int |İlk yeniden deneme girişiminden önce gecikme. Varsayılan değer 1'dir.|
|**maximum_retry_interval_seconds** |int|   Yeniden deneme girişimleri arasındaki en büyük gecikme. Yeniden denemeler arasındaki gecikme bu değerden daha büyük geçerseniz, bunun yerine bu değere tavan sabitlenir. Varsayılan değer 120'dir.|
|**retry_interval_backoff_multiplier**  |Gerçek|  Birden çok iş yürütme adım durumunda yeniden deneme gecikmesi uygulanacak çarpan denemesi başarısız. 2.0 varsayılan değerdir.|
|**retry_attempts** |int|   Yeniden deneme sayısı, bu adım başarısız olursa kullanmaya çalışır. Herhangi bir yeniden deneme girişimleri gösteren varsayılan 10.|
|**step_timeout_seconds**   |int|   Yeniden deneme girişimleri arasındaki dakika cinsinden süre miktarı. Varsayılan değer 0 dakika arayla gösteren 0 ' dır.|
|**output_type**    |nvarchar(11)|  Komut dosyasının konumu. Geçerli Önizleme 'Inline' varsayılandır ve yalnızca kabul edilen değer.|
|**output_credential_name**|    nvarchar(128)   |Sonuçlarını depolamak için hedef sunucuya bağlanmak için kullanılacak kimlik bilgilerini ayarlayın.|
|**output_subscription_id**|    benzersiz tanımlayıcı|   Sonuçları sorgu yürütmeyi ayarlamak için hedef server\database aboneliğini benzersiz kimliği.|
|**output_resource_group_name** |nvarchar(128)| Hedef sunucunun bulunduğu kaynak grubu adı.|
|**output_server_name**|    nvarchar(256)   |Sonuçlar için hedef sunucunun adını ayarlayın.|
|**output_database_name**   |nvarchar(128)| Hedef veritabanı adı sonuçlar için ayarlayın.|
|**output_schema_name** |nvarchar(max)| Hedef şema adı. Dbo belirtilmezse, varsayılan olarak.|
|**output_table_name**| nvarchar(max)|  Sorgu sonuçlarından ayarlayın Sonuçların depolanacağı bir tablonun adı. Tablo, zaten yoksa ayarlamak sonuçlarının şemaya göre otomatik olarak oluşturulur. Şema, sonuç kümesi şeması ile eşleşmesi gerekir.|
|**max_parallelism**|   int|    İş adımı aynı anda üzerinde çalıştırılacak bir elastik havuz başına veritabanı sayısı. Sınırsız anlamına gelir, NULL varsayılandır. |


### <a name="jobstepversions-view"></a>jobstep_versions görüntüle

[iş]. [jobstep_versions]

Tüm adımları her bir iş tüm sürümlerini gösterir. Şema aynıdır [sp_reassign_proxy](#jobsteps-view).

### <a name="targetgroups-view"></a>target_groups görüntüle

[iş]. [target_groups]

Tüm hedef gruplarını listeler.

|Sütun adı|Veri türü| Açıklama|
|-----|-----|-----|
|**target_group_name**| nvarchar(128)   |Hedef grup, veritabanlarının bir koleksiyon adı. 
|**target_group_id**    |benzersiz tanımlayıcı   |Hedef grubun benzersiz kimliği.

### <a name="targetgroupsmembers-view"></a>target_groups_members görüntüle

[iş]. [target_groups_members]

Tüm hedef grupların tüm üyeleri gösterir.

|Sütun adı|Veri türü| Açıklama|
|-----|-----|-----|
|**target_group_name**  |nvarchar (128|Hedef grup, veritabanlarının bir koleksiyon adı. |
|**target_group_id**    |benzersiz tanımlayıcı   |Hedef grubun benzersiz kimliği.|
|**membership_type**    |int|   Hedef grup üyesi dahil veya hedef grupta dışlanan belirtir. 'Include' veya 'Exclude' target_group_name için geçerli değerlerdir.|
|**target_type**    |nvarchar(128)| Hedef veritabanı veya tüm veritabanları bir sunucu, bir elastik havuzdaki tüm veritabanları veya veritabanı gibi veritabanlarının koleksiyon türü. 'SqlServer', 'SqlElasticPool', 'Temel' veya 'SqlShardMap' target_type için geçerli değerlerdir.|
|**target_id**  |benzersiz tanımlayıcı|  Hedef grup üyesi benzersiz kimliği.|
|**refresh_credential_name**    |nvarchar(128)  |Veritabanının adı hedef grubu üyesine bağlanmak için kullanılan kimlik bilgilerini kapsamı.|
|**subscription_id**    |benzersiz tanımlayıcı|  Abonelik benzersiz kimliği.|
|**resource_group_name**    |nvarchar(128)| Hedef grup üyesi bulunduğu kaynak grubunun adı.|
|**SERVER_NAME**    |nvarchar(128)  |Hedef grup içinde bulunan SQL veritabanı sunucusunun adı. Yalnızca target_type 'SqlServer' belirtilmelidir. |
|**database_name**  |nvarchar(128)  |Hedef grup içinde bulunan veritabanının adı. Target_type 'Temel' olduğunda yalnızca belirtilen.|
|**elastic_pool_name**  |nvarchar(128)| Hedef grup içinde bulunan bir elastik havuz adı. Target_type 'SqlElasticPool' olduğunda yalnızca belirtilen.|
|**shard_map_name** |nvarchar(128)| Hedef grup içinde bulunan parça eşlemesi adı. Target_type 'SqlShardMap' olduğunda yalnızca belirtilen.|


## <a name="resources"></a>Kaynaklar

 - ![Konu bağlantı simgesi](https://docs.microsoft.com/sql/database-engine/configure-windows/media/topic-link.gif "konu bağlantı simgesi") [Transact-SQL söz dizimi kuralları](https://docs.microsoft.com/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql)  


## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell’i kullanarak Elastik İşler oluşturma ve yönetme](elastic-jobs-powershell.md)
- [SQL Server yetkilendirme ve izinler](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/authorization-and-permissions-in-sql-server)
