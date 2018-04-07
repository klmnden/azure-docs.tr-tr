---
title: SQL veritabanı için XEvent olay dosyası kodu | Microsoft Docs
description: PowerShell ve Transact-SQL Azure SQL veritabanı genişletilmiş bir olay Olay dosyası hedef gösteren iki aşamalı kod örneği sağlar. Azure depolama, bu senaryo, gerekli bir parçasıdır.
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/01/2018
ms.author: genemi
ms.openlocfilehash: f13ac366a1c382e955db23f3bcefb8f31c89fcb9
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>SQL veritabanı genişletilmiş olaylar için olay dosya hedef kodu

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Genişletilmiş olay yakalama ve rapor bilgilerini için sağlam bir şekilde için tam bir kod örneği istiyor.

Microsoft SQL Server'da [olay dosyası hedef](http://msdn.microsoft.com/library/ff878115.aspx) olay çıkışları bir yerel sabit sürücüye dosyasına depolamak için kullanılır. Ancak bu tür dosyaları Azure SQL veritabanı için kullanılabilir değil. Bunun yerine Azure Storage hizmeti olay dosyası hedef desteklemek için kullanırız.

Bu konuda iki aşamalı kod örneği gösterir:

* PowerShell, bulutta bir Azure depolama kapsayıcısı oluşturulamadı.
* Transact-SQL:
  
  * Bir olay dosyası hedef Azure depolama kapsayıcısının atamak için.
  * Oluşturma ve olay oturumu başlatmak ve benzeri için.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure hesabı ve aboneliği [Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Herhangi bir veritabanı, bir tablodaki oluşturabilirsiniz.
  
  * İsteğe bağlı olarak yapabileceğiniz [oluşturma bir **AdventureWorksLT** demo veritabanı](sql-database-get-started.md) dakika.
* SQL Server Management Studio (ssms.exe), ideally its latest monthly update version. 
  En son ssms.exe öğesinden yükleyebilirsiniz:
  
  * Başlıklı konuda [SQL Server Management Studio'yu indirme](http://msdn.microsoft.com/library/mt238290.aspx).
  * [İndirme doğrudan bağlantı.](http://go.microsoft.com/fwlink/?linkid=616025)
* Bilmeniz gereken [Azure PowerShell modülleri](http://go.microsoft.com/?linkid=9811175) yüklü.
  
  * Modülleri komutları gibi - sağlayan **yeni AzureStorageAccount**.

## <a name="phase-1-powershell-code-for-azure-storage-container"></a>1. Aşama: Azure depolama kapsayıcısı PowerShell kodu

Bu PowerShell iki aşamalı kod örneği Aşama 1 ' dir.

Komut dosyası önceki olası çalıştırın ve rerunnable sonra temizlemek için komutları ile başlar.

1. PowerShell Betiği Notepad.exe gibi basit bir metin düzenleyicisine yapıştırın ve betiği uzantılı bir dosya olarak kaydedin **.ps1**.
2. PowerShell ISE yönetici olarak başlatın.
3. İstemine yazın<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>ve ardından Enter tuşuna basın.
4. PowerShell ISE'yi açın, **.ps1** dosya. Betiği çalıştırın.
5. Komut dosyası, Azure'da oturum açtığınızda yeni bir pencere ilk başlar.
   
   * Oturumunuz kesintiye uğratmadan komut dosyasını yeniden kullanıma yorum uygun seçeneği varsa **Add-AzureAccount** komutu.

![PowerShell ISE, Azure Modülü yüklü, komut dosyasını çalıştırmak hazır.][30_powershell_ise]

### <a name="powershell-code"></a>PowerShell kodu

Bu PowerShell komut dosyasını önceden AzureRm modülü için Import-Module cmdlet'i çalıştırdığınız varsayar. Başvuru belgelerine bakın [PowerShell modülü tarayıcı](https://docs.microsoft.com/powershell/module/).

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

cls;

#--------------- 1 -----------------------

'Script assumes you have already logged your PowerShell session into Azure.
But if not, run  Add-AzureRmAccount (or  Login-AzureRmAccount), just one time.';
#Add-AzureRmAccount;   # Same as  Login-AzureRmAccount.

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

$storageAccountLocation = 'West US';
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

Select-AzureRmSubscription -Subscription $subscriptionName;

#-------------- 4 ------------------------

'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.';

If ($storageAccountName)
{
    Remove-AzureRmStorageAccount `
        -Name              $storageAccountName `
        -ResourceGroupName $resourceGroupName;
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString();

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...';

New-AzureRmStorageAccount `
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
    (Get-AzureRmStorageAccountKey `
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

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $accessKey_ForStorageAccount;

'Create a container within the storage account.
';

$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context;

'Create a security policy to be applied to the SAS token.
';

New-AzureStorageContainerStoredAccessPolicy `
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
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
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
(Later, return here to delete your Azure Storage account. See the preceding  Remove-AzureRmStorageAccount -Name $storageAccountName)';

'
Now shift to the Transact-SQL portion of the two-part code sample!';

# EOFile
```


Sona erdiğinde, PowerShell Betiği yazdıran birkaç adlandırılmış değerlerini not edin. Aşama 2 aşağıdaki Transact-SQL komut dosyası içine bu değerleri düzenlemeniz gerekir.

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>2. Aşama: Azure Storage kapsayıcısını kullanan Transact-SQL kodu

* Bir Azure depolama kapsayıcısı oluşturmak için bir PowerShell Betiği çalıştırdığınız aşamada Bu kod örneği, 1.
* Sonraki aşama 2'de, aşağıdaki Transact-SQL komut dosyası kapsayıcı kullanmanız gerekir.

Komut dosyası önceki olası çalıştırın ve rerunnable sonra temizlemek için komutları ile başlar.

PowerShell Betiği birkaç adlandırılmış değerler yazdırıldığında bunu sona erdi. Bu değerleri kullanmak için Transact-SQL komut dosyasını düzenlemeniz gerekir. Bul **Yapılacaklar** düzenleme noktaları bulmak için Transact-SQL komut.

1. Open SQL Server Management Studio (ssms.exe).
2. Azure SQL veritabanı veritabanınızla bağlantı kurun.
3. Yeni bir sorgu bölmesini açmak için tıklatın.
4. Aşağıdaki Transact-SQL betiği sorgu bölmesine yapıştırın.
5. Bulma her **Yapılacaklar** komut ve uygun düzenlemeleri yapın.
6. Kaydedin ve ardından komut dosyasını çalıştırın.


> [!WARNING]
> Önceki PowerShell betiği tarafından oluşturulan SAS anahtar değeri ile başlayabilir bir '?' (soru işareti). SAS anahtarını aşağıdaki T-SQL komut dosyasında kullandığınızda, şunları yapmalısınız *kaldırmak başında '?'* . Aksi takdirde çabalarınız güvenliği tarafından engellenmiş olabilir.


### <a name="transact-sql-code"></a>Transact-SQL kodunu

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

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


Çalıştırdığınızda eklemek hedef başarısız olursa, durdurmak ve olay oturumu yeniden başlatın:

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a>Çıktı

Transact-SQL betiği tamamlandığında altında bir hücreyi tıklatın **event_data_XML** sütun başlığı. Bir **<event>** öğesi bir güncelleştirme deyim gösteren görüntülenir.

Bir işte **<event>** test sırasında oluşturulan öğe:


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

* [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)

Genişletilmiş olay verilerini görüntülemek için Gelişmiş Seçenekleri açıklaması şu adresten edinilebilir:

* [Gelişmiş genişletilmiş olaylar hedef verilerini görüntüleme](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>SQL Server üzerinde çalıştırmak için kod örneği dönüştürme

Microsoft SQL Server üzerinde önceki Transact-SQL örneği çalıştırmak istediğinizi varsayalım.

* Kolaylık olması için tamamen Azure depolama kapsayıcısının kullanımını basit bir dosyayla gibi değiştirmek istiyor **C:\myeventdata.xel**. Dosya, SQL Server'ı barındıran bilgisayarın yerel sabit sürücüye yazılacaktır.
* Transact-SQL deyimi için herhangi bir tür ihtiyaç duymaz **CREATE MASTER KEY** ve **kimlik bilgisi Oluştur**.
* İçinde **oluşturma olay OTURUMU** deyimi içinde kendi **hedef Ekle** yan tümcesi, atanan Http değerini değiştirmek yapılan **filename =** gibi bir tam yol dizesini **C:\myfile.xel**.
  
  * Azure depolama hesabı söz konusu.

## <a name="more-information"></a>Daha fazla bilgi

Hesapları ve Azure Storage hizmetinde kapsayıcıları hakkında daha fazla bilgi için bkz:

* [BLOB depolama alanından .NET kullanma](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [Kök kapsayıcı ile çalışma](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [Ders 1: bir Azure kapsayıcısı üzerinde depolanan erişim ilkesi ve bir paylaşılan erişim imzası oluşturma](http://msdn.microsoft.com/library/dn466430.aspx)
  * [Ders 2: paylaşılan erişim imzası kullanarak bir SQL Server kimlik bilgisi oluşturma](http://msdn.microsoft.com/library/dn466435.aspx)
* [Microsoft SQL Server için genişletilmiş olaylar](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

