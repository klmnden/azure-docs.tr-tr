---
title: "Hızlı Başlangıç: Azure SQL Data Warehouse'da - PowerShell duraklatma ve sürdürme işlem | Microsoft Docs"
description: Duraklatma işlem maliyetleri kaydetmek için Azure SQL Data warehouse'da PowerShell kullanın. Veri ambarı kullanılmaya hazır olduğunuzda sürdürebilirsiniz.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: ef341a1528bf759461abfb7cfc6d878fd8a44cb4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31598906"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-sql-data-warehouse-with-powershell"></a>Hızlı Başlangıç: PowerShell ile Azure SQL Data Warehouse, duraklatma ve sürdürme işlem
Duraklatma işlem maliyetleri kaydetmek için Azure SQL Data warehouse'da PowerShell kullanın. [İşlem devam](sql-data-warehouse-manage-compute-overview.md) veri ambarı kullanılmaya hazır olduğunuzda.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu öğreticide Azure PowerShell modülü sürümü 5.1.1 gerektirir veya sonraki bir sürümü. Çalıştırma ` Get-Module -ListAvailable AzureRM` sürümünü bulmak için şu anda yok. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu Hızlı Başlangıç, duraklatma ve sürdürme SQL data warehouse zaten olduğunu varsayar. Bir oluşturmanız gerekiyorsa, kullanabileceğiniz [oluşturma ve Connect - portal](create-data-warehouse-portal.md) adlı bir veri ambarı oluşturmak için **mySampleDataWarehouse**.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
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
3. Seçin **mySampleDataWarehouse** gelen **SQL veritabanları** sayfası. Veri ambarı açar.

    ![Sunucu adı ve kaynak grubu](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Veritabanı adı veri ambarı adını yazın. Ayrıca sunucu adını ve kaynak grubunu not alın. ,
5.  Bu duraklatma ve sürdürme komutlarını.
6. Sunucunuz foo.database.windows.net ise, PowerShell cmdlet'leri sunucu adı olarak yalnızca ilk bölümü kullanın. Önceki görüntüde newserver 20171113.database.windows.net tam sunucu adıdır. Sonek bırakın ve kullanmak **newserver 20171113** PowerShell cmdlet sunucu adı olarak.

## <a name="pause-compute"></a>Duraklatma işlem
Maliyet tasarrufu sağlamak duraklatma ve sürdürme işlem kaynakları isteğe bağlı. Örneğin, gece sırasında ve hafta sonları veritabanı kullanmıyorsanız, bu saatlerde duraklatma ve gün boyunca sürdürün. Veritabanı durdurulduğunda ücret ödemeden işlem kaynakları yoktur. Ancak, depolama için sizden ücret devam.

Bir veritabanı duraklatmak için kullanmak [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase.md) cmdlet'i. Aşağıdaki örnek adlı bir veri ambarı duraklatır **mySampleDataWarehouse** adlı bir sunucuda barındırılan **newserver 20171113**. Adlı bir Azure kaynak grubu içinde sunucusudur **myResourceGroup**.


```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
```

Bir değişim bu sonraki örnek veritabanı $database nesnesine alır. Sonra nesneyi kanallar [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase). Sonuçlar nesne resultDatabase içinde depolanır. Son komutun sonuçlarını gösterir.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```


## <a name="resume-compute"></a>Resume işlem
Bir veritabanı başlatmak için kullanmak [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase) cmdlet'i. Aşağıdaki örnek newserver 20171113 adlı bir sunucuda barındırılan mySampleDataWarehouse adlı bir veritabanı başlatır. Sunucu myResourceGroup adlı bir Azure kaynak grubunda bulunuyor.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" -DatabaseName "mySampleDataWarehouse"
```

Bir değişim bu sonraki örnek veritabanı $database nesnesine alır. Sonra nesneyi kanallar [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase.md) ve sonuçları $resultDatabase içinde depolar. Son komutun sonuçlarını gösterir.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve veri ambarınızda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Veri deposunda tutmak istiyorsanız, işlem duraklatın.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz.

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. Oturum [Azure portal](https://portal.azure.com)ve veri ambarınız'ı tıklatın.

    ![Kaynakları temizleme](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlemi sürdürmek için **Başlat**’a tıklayın.

2. İşlem veya depolama için ücretlendirilmemek üzere veri ambarını kaldırmak için **Sil**’e tıklayın.

3. Oluşturduğunuz SQL server kaldırmak için tıklatın **mynewserver 20171113.database.windows.net**ve ardından **silmek**.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

4. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
Şimdi duraklatıldı sahip ve işlem için veri ambarınız sürdürüldü. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
