---
title: "DNS diğer adı Azure SQL için PowerShell | Microsoft Docs"
description: "PowerShell cmdlet'leri yeni AzureRMSqlServerDNSAlias gibi yeni istemci bağlantılarını herhangi bir istemci yapılandırma touch gerek kalmadan, farklı bir Azure SQL veritabanı sunucusuna yeniden yönlendirmek sağlar."
keywords: "DNS sql veritabanı"
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.service: sql-database
ms.custom: 
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/05/2018
ms.reviewer: genemi;amagarwa;maboja
ms.author: dmalik
ms.openlocfilehash: ec638d7b48b443cda5755e3077c6304b0c5ad78e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>Azure SQL veritabanı için DNS diğer adı için PowerShell

Bu makalede Azure SQL veritabanı için bir DNS diğer adı nasıl yönetebileceğinizi gösterir bir PowerShell komut dosyası sağlar. Komut dosyasını aşağıdaki eylemleri gerçekleştirir aşağıdaki cmdlet'lerini çalıştırır:


Aşağıdaki kod örneğinde kullanılan cmdlet'ler şunlardır:
- [AzureRMSqlServerDNSAlias yeni](https://docs.microsoft.com/powershell/module/AzureRM.Sql/New-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Azure SQL veritabanı hizmetinin sistemde yeni bir DNS diğer adı oluşturur. Diğer Azure SQL veritabanı sunucusuna 1 başvuruyor.
- [Get-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): almak ve 1 SQL DB sunucusuna atanan tüm DNS diğer adları listesi.
- [Set-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Set-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): diğer adı için yapılandırılmış sunucu adını değiştirir, SQL veritabanı sunucusuna 2 1 sunucusundan bakın.
- [Remove-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Remove-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Remove the DNS alias from SQL DB server 2, by using the name of the alias.


Önceki PowerShell cmdlet'leri eklenmiştir **AzureRm.Sql** 5.1.1 sürümünden başlayarak modülü.




#### <a name="dns-alias-in-connection-string"></a>DNS diğer adı bağlantı dizesinde

Belirli bir Azure SQL veritabanı sunucusuna bağlanmak için SQL Server Management Studio (SSMS) gibi istemci DNS diğer adı gerçek sunucu adı yerine sağlayabilir. Aşağıdaki örnek sunucu dizesindeki diğer *herhangi-benzersiz-diğer ad* dört düğümlü sunucu dize ilk nokta ayrılmış düğüm değiştirir:
- Örnek sunucu dize: `any-unique-alias-name.database.windows.net`.



## <a name="prerequisites"></a>Önkoşullar

Bu makalede verilen PowerShell Betiği demo çalıştırmak istiyorsanız, aşağıdaki önkoşulları Uygula:

- Bir Azure aboneliği ve hesabı. Ücretsiz deneme için tıklatın [https://azure.microsoft.com/free/][https://azure.microsoft.com/free/].

- Cmdlet ile Azure PowerShell Modülü **yeni AzureRMSqlServerDNSAlias**.
    - Yüklemek veya yükseltmek için bkz: [yükleme Azure PowerShell Modülü][install-azurerm-ps-84p].
    - Çalıştırma `Get-Module -ListAvailable AzureRM;` PowerShell'de\_sürümünü bulmak için ise.exe.

- İki Azure SQL veritabanı sunucusu.

## <a name="code-example"></a>Kod örneği

Kod örneği tarafından başlatır aşağıdaki PowerShell çeşitli değişkenler değişmez değerler atayın. Kodu çalıştırmak için sisteminizde gerçek değerlerin eşleşmesi için tüm yer tutucu değerlerini düzenlemeniz gerekir. Veya yalnızca kodu araştırmak. Ve kod konsol çıkışı de sağlanır.


```powershell
################################################################
###    Assign prerequisites.                                 ###
################################################################
cls;

$SubscriptionName             = '<EDIT-your-subscription-name>';
[string]$SubscriptionGuid_Get = '?'; # The script assigns this value, not you.

$SqlServerDnsAliasName = '<EDIT-any-unique-alias-name>';

$1ResourceGroupName = '<EDIT-rg-1>';  # Can be same or different than $2ResourceGroupName.
$1SqlServerName     = '<EDIT-sql-1>'; # Must differ from $2SqlServerName.

$2ResourceGroupName = '<EDIT-rg-2>';
$2SqlServerName     = '<EDIT-sql-2>';

# Login to your Azure subscription, first time per session.
Write-Host "You must log into Azure once per powershell_ise.exe session,";
Write-Host "  thus type 'yes' only the first time.";
Write-Host " ";
$yesno = Read-Host '[yes/no]  Do you need to log into Azure now?';
if ('yes' -eq $yesno)
{
    Login-AzureRmAccount -SubscriptionName $SubscriptionName;
}

$SubscriptionGuid_Get = Get-AzureRmSubscription `
    -SubscriptionName $SubscriptionName;

################################################################
###    Working with DNS aliasing for Azure SQL DB server.    ###
################################################################

Write-Host '[1] Assign a DNS alias to SQL DB server 1.';
New-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $1ResourceGroupName `
    -ServerName         $1SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;

Write-Host '[2] Get and list all the DNS aliases that are assigned to SQL DB server 1.';
Get-AzureRMSqlServerDNSAlias `
    –ResourceGroupName $1ResourceGroupName `
    -ServerName        $1SqlServerName;

Write-Host '[3] Move the DNS alias from 1 to SQL DB server 2.';
Set-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -NewServerName      $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName `
    -OldServerResourceGroup  $1ResourceGroupName `
    -OldServerName           $1SqlServerName `
    -OldServerSubscriptionId $SubscriptionGuid_Get;

# Here your client, such as SSMS, can connect to your "$2SqlServerName"
# by using "$SqlServerDnsAliasName" in the server name.
# For example, server:  "any-unique-alias-name.database.windows.net".

# Remove-AzureRMSqlServerDNSAlias  - would fail here for SQL DB server 1.

Write-Host '[4] Remove the DNS alias from SQL DB server 2.';
Remove-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -ServerName         $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;
```


#### <a name="actual-console-output-from-the-powershell-example"></a>PowerShell örnek gerçek konsol çıktısı

Aşağıdaki konsol çıktısı kopyalanır ve gerçek bir çalıştırma yapıştırılan.


```
You must log into Azure once per powershell_ise.exe session,
  thus type 'yes' only the first time.
 
[yes/no]  Do you need to log into Azure now?: yes


Environment           : AzureCloud
Account               : gm@acorporation.com
TenantId              : 72f988bf-1111-1111-1111-111111111111
SubscriptionId        : 45651c69-2222-2222-2222-222222222222
SubscriptionName      : mysubscriptionname
CurrentStorageAccount : 

 
[1] Assign a DNS alias to SQL DB server 1.
[2] Get the DNS alias that is assigned to SQL DB server 1.
[3] Move the DNS alias from 1 to SQL DB server 2.
[4] Remove the DNS alias from SQL DB server 2.
ResourceGroupName ServerName         ServerDNSAliasName    
----------------- ----------         ------------------    
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-2       gm-sqldb-dns-2     unique-alias-name-food


[C:\windows\system32\]
>> 
```

## <a name="next-steps"></a>Sonraki adımlar

Bir tam açıklamasını DNS diğer adı özelliği için SQL veritabanı için bkz: [Azure SQL veritabanı için DNS diğer adı][dns-alias-overview-37v].



<!-- Article links. -->

[https://azure.microsoft.com/free/]: https://azure.microsoft.com/free/

[install-azurerm-ps-84p]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps

[dns-alias-overview-37v]: dns-alias-overview.md

