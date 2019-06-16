---
title: SQL veritabanı için XEvent olay dosyası kod | Microsoft Docs
description: Olay dosyası hedef Azure SQL veritabanı'nda genişletilmiş bir olay gösteren iki aşamalı bir kod örneği için PowerShell ve Transact-SQL sağlar. Azure depolama, bu senaryo, gerekli bir parçasıdır.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: PowerShell
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: jrasnik
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: ce559e50d5a34ebad9113f0e21dcb732adc40dd2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233772"
---
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>SQL veritabanı'nda genişletilmiş olaylar için olay dosyası hedef kodu

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Genişletilmiş olay yakalama ve rapor bilgilerini için güçlü bir yol için bir kod örneği istersiniz.

Microsoft SQL Server'da [olay dosyası hedef](https://msdn.microsoft.com/library/ff878115.aspx) bir yerel sabit diske dosyasına olay çıktılarının depolanması için kullanılır. Ancak, bu tür dosyaları Azure SQL veritabanı'na kullanılabilir değil. Bunun yerine Azure depolama hizmeti olay dosyası hedef desteklemek için kullanırız.

Bu konuda, bir iki aşamalı bir kod örneği sunar:

* PowerShell, bulutta bir Azure depolama kapsayıcısı oluşturmak için kullanılır.
* Transact-SQL:
  
  * Azure depolama kapsayıcısı için bir olay dosyası hedef atamak için.
  * Oluşturma ve olay oturumu başlatın ve benzeri için.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

* Bir Azure hesabı ve aboneliği [Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Herhangi bir veritabanı içinde bir tablo oluşturabilirsiniz.
  
  * İsteğe bağlı olarak yapabilecekleriniz [oluşturmak bir **AdventureWorksLT** demo veritabanı](sql-database-get-started.md) dakikalar içinde.
* SQL Server Management Studio (ssms.exe), ideal olarak en son aylık güncelleştirme sürümü. 
  Gelen son ssms.exe indirebilirsiniz:
  
  * Başlıklı konusuna [SQL Server Management Studio'yu indirme](https://msdn.microsoft.com/library/mt238290.aspx).
  * [İndirme için doğrudan bir bağlantı.](https://go.microsoft.com/fwlink/?linkid=616025)
* Olmalıdır [Azure PowerShell modüllerini](https://go.microsoft.com/?linkid=9811175) yüklü.
  
  * Modüller komutları gibi - sağlayan **yeni AzStorageAccount**.

## <a name="phase-1-powershell-code-for-azure-storage-container"></a>1\. Aşama: Azure depolama kapsayıcısı için PowerShell kodu

1\. Aşama iki aşamalı kod örneğinin powershell'dir.

Betik, olası bir önceki çalıştırma ve rerunnable sonra temizlemek için komutları ile başlar.

1. Notepad.exe gibi bir metin düzenleyicisinde PowerShell betiğini yapıştırın ve betiği uzantılı bir dosya olarak kaydedin **.ps1**.
2. PowerShell ISE yönetici olarak başlatın.
3. İsteminde<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>' i tıklatın ve ardından Enter tuşuna basın.
4. PowerShell ISE'de açın, **.ps1** dosya. Betiği çalıştırın.
5. Komut içinde Azure'da oturum açarken yeni bir pencere ilk başlatır.
   
   * Oturumunuz kesintiye uğratmadan betiği yeniden çalıştırın, out yorum kullanışlı seçeneğiniz vardır **Add-AzureAccount** komutu.

![PowerShell ISE, Azure Modülü yüklü, komut dosyasını çalıştırmak hazır.][30_powershell_ise]

### <a name="powershell-code"></a>PowerShell kodu

Bu PowerShell Betiği, Az modül zaten yüklü olduğunu varsayar. Bilgi için [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps).

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

cls;

#--------------- 1 -----------------------

'Script assumes you have already logged your PowerShell session into Azure.
But if not, run  Connect-AzAccount (or  Connect-AzAccount), just one time.';
#Connect-AzAccount;   # Same as  Connect-AzAccount.

#-------------- 2 ------------------------

'
TODO: Edit the values assigned to these variables, especially the first few!
';

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME';
$resourceGroupName   = 'YOUR_RESOURCE-GROUP-NAME';

$policySasExpiryTime = '2018-08-28T23:44:56Z';
$policySasStartTime  = '2017-10-01';

$storageAccountLocation = 'YOUR_STORAGE_ACCOUNT_LOCATION';
$storageAccountName     = 'YOUR_STORAGE_ACCOUNT_NAME';
$contextName            = 'YOUR_CONTEXT_NAME';
$containerName          = 'YOUR_CONTAINER_NAME';
$policySasToken         = ' ? ';

$policySasPermission = 'rwl';  # Leave this value alone, as 'rwl'.

#--------------- 3 -----------------------

# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.';

Select-AzSubscription -Subscription $subscriptionName;

#-------------- 4 ------------------------

'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.';

If ($storageAccountName)
{
    Remove-AzStorageAccount `
        -Name              $storageAccountName `
        -ResourceGroupName $resourceGroupName;
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString();

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...';

New-AzStorageAccount `
    -Name              $storageAccountName `
    -Location          $storageAccountLocation `
    -ResourceGroupName $resourceGroupName `
    -SkuName           'Standard_LRS';

[System.DateTime]::Now.ToString();
[System.Media.SystemSounds]::Beep.Play();

'
Get the access key for your storage account.
';

$accessKey_ForStorageAccount = `
    (Get-AzStorageAccountKey `
        -Name              $storageAccountName `
        -ResourceGroupName $resourceGroupName
        ).Value[0];

"`$accessKey_ForStorageAccount = $accessKey_ForStorageAccount";

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
';

#--------------- 6 -----------------------

# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
';

$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $accessKey_ForStorageAccount;

'Create a container within the storage account.
';

$containerObjectInStorageAccount = New-AzStorageContainer `
    -Name    $containerName `
    -Context $context;

'Create a security policy to be applied to the SAS token.
';

New-AzStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime;

'
Generate a SAS token for the container.
';
Try
{
    $sasTokenWithPolicy = New-AzStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken;
}
Catch 
{
    $Error[0].Exception.ToString();
}

#-------------- 7 ------------------------

'Display the values that YOU must edit into the Transact-SQL script next!:
';

"storageAccountName: $storageAccountName";
"containerName:      $containerName";
"sasTokenWithPolicy: $sasTokenWithPolicy";

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
';

'
(Later, return here to delete your Azure Storage account. See the preceding  Remove-AzStorageAccount -Name $storageAccountName)';

'
Now shift to the Transact-SQL portion of the two-part code sample!';

# EOFile
```


PowerShell betiğini sona erdiğinde yazdıran birkaç adlandırılmış değerleri not alın. Bu değerler, Aşama 2 aşağıdaki Transact-SQL komut dosyası içine düzenlemeniz gerekir.

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>2\. Aşama: Azure depolama kapsayıcısı kullanan transact-SQL kodu

* Bu kod örneği, 1 aşamasında, bir Azure depolama kapsayıcısı oluşturmak için bir PowerShell Betiği çalıştırdınız.
* Sonraki aşama 2'de, aşağıdaki Transact-SQL betiğini kapsayıcı kullanmanız gerekir.

Betik, olası bir önceki çalıştırma ve rerunnable sonra temizlemek için komutları ile başlar.

PowerShell Betiği, ne zaman bittiğini birkaç adlandırılmış değerler yazdırılan. Bu değerleri kullanmak için Transact-SQL betiğini düzenlemeniz gerekir. Bulma **TODO** düzenleme noktaları bulmak için Transact-SQL komut.

1. SQL Server Management Studio'yu (ssms.exe) açın.
2. Azure SQL veritabanı veritabanınıza bağlanın.
3. Yeni bir sorgu bölmesini açmak için tıklayın.
4. Aşağıdaki Transact-SQL betiğini sorgu bölmesine yapıştırın.
5. Bulma her **TODO** komut ve uygun düzenlemeleri yapın.
6. Kaydedin ve ardından komut dosyasını çalıştırın.


> [!WARNING]
> Önceki PowerShell betiği tarafından oluşturulan SAS anahtarının değeri ile başlayabilir bir '?' (soru işareti). Aşağıdaki T-SQL betiği SAS anahtarını kullanırken, şunları yapmalısınız *kaldırmak başında '?'* . Aksi takdirde çalışmalarınızı security tarafından engelleniyor olabilir.


### <a name="transact-sql-code"></a>Transact-SQL kodu

```sql
---- TODO: First, run the earlier PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                -- TODO: The name of the .xel file needs to be an exact match to the files in the storage account Container (You can use Storage Account explorer from the portal to find out the exact file names or you can retrieve the name using the following DMV-query: select target_data from sys.dm_xe_database_session_targets. The 3rd xml-node, "File name", contains the name of the file currently written to.)
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzStorageAccount to delete your Azure Storage account!';
GO
```


Programını çalıştırdığınızda eklemek hedef başarısız olursa, durdurun ve olay oturumu yeniden başlatın:

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a>Çıktı

Transact-SQL betik tamamlandığında, altında bir hücreyi tıklatın **event_data_XML** sütun başlığı. Bir  **\<olay >** öğeyi gösteren bir güncelleştirme bildirimi görüntülenir.

İşte  **\<olay >** testi sırasında oluşturulan öğesi:


```xml
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```


Önceki Transact-SQL betiği aşağıdaki sistem işlevi event_file okumak için kullanılır:

* [sys.fn_xe_file_target_read_file (Transact-SQL)](https://msdn.microsoft.com/library/cc280743.aspx)

Genişletilmiş olaylar verileri Gelişmiş seçenekleri görüntülemek için bir açıklama şuradan ulaşabilirsiniz:

* [Genişletilmiş olaylar hedef verileri gelişmiş görüntüleme](https://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Kod örneği, SQL Server üzerinde çalıştırmak için dönüştürme

Microsoft SQL Server'da önceki Transact-SQL örneği çalıştırmak istediğinizi varsayalım.

* Kolaylık olması için tamamen Azure depolama kapsayıcısı kullanımı gibi basit bir dosya ile değiştirmek istiyorsunuz **C:\myeventdata.xel**. Dosya, SQL Server'ı barındıran bilgisayarın yerel sabit sürücüye yazılacaktır.
* Sizin için Transact-SQL deyimleriyle herhangi bir türden ihtiyaç duymaz **CREATE MASTER KEY** ve **kimlik bilgisi oluşturma**.
* İçinde **olay OTURUMU oluşturma** deyimi, kendi **ekleme hedef** yan tümcesi atanır Http değeri değiştirmek yapılan **filename =** gibibirtamyoldizesiile**C:\myfile.xel**.
  
  * Azure depolama hesabı dahil.

## <a name="more-information"></a>Daha fazla bilgi

Hesapları ve Azure depolama hizmetinde kapsayıcıları hakkında daha fazla bilgi için bkz:

* [Net'ten BLOB storage kullanma](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://msdn.microsoft.com/library/azure/dd135715.aspx)
* [Kök kapsayıcı ile çalışma](https://msdn.microsoft.com/library/azure/ee395424.aspx)
* [1. Ders: Bir Azure kapsayıcısı üzerinde depolanmış erişim ilkesini ve bir paylaşılan erişim imzası oluşturma](https://msdn.microsoft.com/library/dn466430.aspx)
  * [2. Ders: Paylaşılan erişim imzası kullanarak bir SQL Server kimlik bilgileri oluşturma](https://msdn.microsoft.com/library/dn466435.aspx)
* [Microsoft SQL Server için genişletilmiş olaylar](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

