---
title: "Hızlı Başlangıç: Ölçek genişletme Azure SQL Data Warehouse - PowerShell işlem | Microsoft Docs"
description: "Data warehouse birimleri ayarlayarak işlem kaynaklarını genişletmek için Powershell görevleri."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/31/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: ff6c14ced42385d81f3c42b4b4bf1fd06464c0d1
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-powershell"></a>Hızlı Başlangıç: Azure SQL Data warehouse'da PowerShell'de işlem ölçeklendirme

Azure SQL Data warehouse'da PowerShell'de ölçek işlem. İşlem daha iyi performans için genişletme veya ölçek geri işlem maliyet tasarrufu sağlamak. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu öğreticide Azure PowerShell modülü sürümü 5.1.1 gerektirir veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` sürümünü bulmak için şu anda yok. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps.md). 

## <a name="before-you-begin"></a>Başlamadan önce

Bu hızlı başlangıç ölçeklendirmek SQL data warehouse zaten olduğunu varsayar. Bir oluşturmanız gerekiyorsa, kullanın [oluşturma ve Connect - portal](create-data-warehouse-portal.md) adlı bir veri ambarı oluşturmak için **mySampleDataWarehouse**. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Add-AzureRmAccount
```

Kullanmakta olduğunuz abonelik görmek için çalıştırın [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

```powershell
Get-AzureRmSubscription
```

Varsayılandan farklı bir abonelik kullanmanız gerekiyorsa, çalıştırmak [Select-AzureRmSubscription](/powershell/module/azurerm.profile/select-azurermsubscription).

```powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Veri ambarı bilgileri Ara

Veritabanı adı, sunucu adını ve kaynak grubu, duraklatma ve sürdürme planladığınız veri ambarı için bulun. 

Veri ambarınız için konum bilgilerini bulmak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Tıklatın **SQL veritabanları** Azure portalının sol sayfasındaki.
3. Seçin **mySampleDataWarehouse** gelen **SQL veritabanları** sayfası. Bu, veri ambarı açar. 

    ![Sunucu adı ve kaynak grubu](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Veritabanı adı olarak kullanılacak veri ambarı adını yazın. Ayrıca sunucu adını ve kaynak grubunu not alın. Bu Duraklat kullanabilir ve komutları sürdürün.
5. Sunucunuz foo.database.windows.net ise, PowerShell cmdlet'leri sunucu adı olarak yalnızca ilk bölümü kullanın. Önceki görüntüde newserver 20171113.database.windows.net tam sunucu adıdır. Kullanırız **newserver 20171113** PowerShell cmdlet sunucu adı olarak.

## <a name="scale-compute"></a>Bilgi işlem

SQL veri ambarı'nda artırın veya işlem kaynakları data warehouse birimleri ayarlayarak azaltın. [Oluşturma ve Connect - portal](create-data-warehouse-portal.md) oluşturulan **mySampleDataWarehouse** ve 400 Dwu ile başlatıldı. Dwu için aşağıdaki adımları ayarlamak **mySampleDataWarehouse**.

Data warehouse birimleri değiştirmek için kullanın [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) PowerShell cmdlet'i. Aşağıdaki örnek data warehouse birimleri için DW300 veritabanı için ayarlar **mySampleDataWarehouse** kaynak grubunda barındırılan **myResourceGroup** sunucuda  **mynewserver 20171113**.

```Powershell
Set-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
```

## <a name="check-database-state"></a>Veritabanı durumunu kontrol edin

Veri ambarı geçerli durumunu görmek için [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) PowerShell cmdlet'i. Bu durumunu alır **mySampleDataWarehouse** ResourceGroup veritabanında **myResourceGroup** ve sunucu **mynewserver 20171113.database.windows.net**.

```powershell
Get-AzureRmSqlDatabase -ResourceGroupName myResourceGroup -ServerName mynewserver-20171113 -DatabaseName mySampleDataWarehouse
```

Hangi şöyle bir şey neden olur:

```powershell   
ResourceGroupName             : myResourceGroup
ServerName                    : mynewserver-20171113
DatabaseName                  : mySampleDataWarehouse
Location                      : North Europe
DatabaseId                    : 34d2ffb8-b70a-40b2-b4f9-b0a39833c974
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1_General_CP1_CI_AS
CatalogCollation              :
MaxSizeBytes                  : 263882790666240
Status                        : Online
CreationDate                  : 11/20/2017 9:18:12 PM
CurrentServiceObjectiveId     : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
CurrentServiceObjectiveName   : DW300
RequestedServiceObjectiveId   : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           :
Tags                          :
ResourceId                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
                                resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/mynewserver-20171113/databases/mySampleDataWarehouse
CreateMode                    :
ReadScale                     : Disabled
ZoneRedundant                 : False
```

Ardından görmek için denetleyebilirsiniz **durum** veritabanı. Bu durumda, bu veritabanının çevrimiçi olduğunu görebilirsiniz.  Bu komutu çalıştırdığınızda, çevrimiçi, duraklatma, devam ettirmek, ölçeklendirme veya duraklatıldı durum değeri almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi, veri ambarı için işlem ölçeklendirmek öğrendiniz. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
