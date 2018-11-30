---
title: 'Hızlı Başlangıç: Azure SQL Veri Ambarı’nda işlem ölçeğini genişletme - PowerShell | Microsoft Docs'
description: PowerShell’den Azure SQL Veri Ambarı’nda işlemi ölçeklendirin. Daha iyi performans için işlem ölçeğini genişletin veya maliyet tasarrufu için işlem ölçeğini daraltın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: cb6153d2859968a02345cfbcfa66bd9a4177d864
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52427444"
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-powershell"></a>Hızlı Başlangıç: PowerShell’den Azure SQL Veri Ambarı’nda işlemi ölçeklendirme

PowerShell’den Azure SQL Veri Ambarı’nda işlemi ölçeklendirin. Daha iyi performans için [işlem ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu için işlem ölçeğini daraltın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu öğretici için Azure PowerShell modülünün 5.1.1 veya daha sonraki bir sürümü gerekir. Şu anda kullandığınız sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Başlamadan önce

Bu hızlı başlangıçta zaten ölçeklendirebileceğiniz bir SQL veri ambarına sahip olduğunuz varsayılır. Gerekiyorsa **mySampleDataWarehouse** adlı bir veri ambarı oluşturmak için [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanabilirsiniz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri uygulayın.

```powershell
Connect-AzureRmAccount
```

Kullanmakta olduğunuz aboneliği görmek için [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription) komutunu çalıştırın.

```powershell
Get-AzureRmSubscription
```

Varsayılandan farklı bir abonelik kullanmanız gerekiyorsa, çalıştırma [Set-AzureRmContext](/powershell/module/azurerm.profile/set-azurermcontext).

```powershell
Set-AzureRmContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Veri ambarı bilgilerini arama

Duraklatmayı ve sürdürmeyi planladığınız veri ambarı için veritabanı adını, sunucu adını ve kaynak grubunu bulun.

Veri ambarınız için konum bilgilerini bulmak amacıyla aşağıdaki adımları uygulayın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portalının sol taraftaki sayfasında **SQL veri ambarları**’na tıklayın.
3. **SQL veri ambarları** sayfasından **mySampleDataWarehouse** seçeneğini belirleyin. Bu, veri ambarını açar.

    ![Sunucu adı ve kaynak grubu](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Veritabanı adı olarak kullanılacak olan veri ambarı adını not alın. Veri ambarının tek bir veritabanı türü olduğunu unutmayın. Ayrıca sunucu adını ve kaynak grubunu da not alın. Duraklatma ve sürdürme komutlarında bunları kullanacaksınız.
5. Sunucunuz foo.database.windows.net ise, PowerShell cmdlet'lerinde sunucu adı olarak yalnızca ilk bölümü kullanın. Önceki görüntüde tam sunucu adı newserver-20171113.database.windows.net şeklindedir. PowerShell cmdlet’inde **newserver-20180430** sunucu adını kullanırız.

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

SQL Veri Ambarı’nda, veri ambarı birimlerini ayarlayarak işlem kaynaklarını artırabilir veya azaltabilirsiniz. [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünde **mySampleDataWarehouse** oluşturuldu ve 400 DWU ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) PowerShell cmdlet'ini kullanın. Aşağıdaki örnekte **mynewserver-20180430** sunucusundaki **myResourceGroup** Kaynak grubunda barındırılan **mySampleDataWarehouse** veritabanı için veri ambarı birimleri DW300 olarak ayarlanır.

```Powershell
Set-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
```

## <a name="check-data-warehouse-state"></a>Veri ambarı durumunu denetleme

Veri ambarının geçerli durumunu görmek için [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) PowerShell cmdlet’ini kullanın. Bu, **myResourceGroup** Kaynak Grubundaki ve **mynewserver-20180430.database.windows.net** sunucusundaki **mySampleDataWarehouse** veritabanının durumunu alır.

```powershell
$database = Get-AzureRmSqlDatabase -ResourceGroupName myResourceGroup -ServerName mynewserver-20171113 -DatabaseName mySampleDataWarehouse
$database
```

Bu da aşağıdaki gibi bir sonuç verir:

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

Çıkışta veritabanının **Durumunu** görüntüleyebilirsiniz. Bu durumda, bu veritabanının çevrimiçi olduğunu görebilirsiniz.  Bu komutu çalıştırdığınızda, Çevrimiçi, Duraklatılıyor, Sürdürülüyor, Ölçeklendiriliyor veya Duraklatıldı Durum değerlerinden birini alırsınız.

Durumun kendisini görüntülemek için şu komutu kullanın:

```powershell
$database | Select-Object DatabaseName,Status
```

## <a name="next-steps"></a>Sonraki adımlar
Artık veri ambarınız için işlemin nasıl ölçeklendirileceğini öğrendiniz. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
