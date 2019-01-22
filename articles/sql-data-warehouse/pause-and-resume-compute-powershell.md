---
title: "Hızlı Başlangıç: İşlem duraklatma ve sürdürme, Azure SQL veri ambarı'nda - PowerShell | Microsoft Docs"
description: Maliyetlerden tasarruf etmek için Azure SQL veri ambarı'nda duraklatma işlem PowerShell kullanın. Veri ambarı kullanılmaya hazır olduğunda sürdürebilirsiniz.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 9d2836f49fb92ab13f8e4170f2aab044c810cf3c
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54437165"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-sql-data-warehouse-with-powershell"></a>Hızlı Başlangıç: PowerShell ile Azure SQL veri ambarı'nda işlem duraklatma ve sürdürme
Maliyetlerden tasarruf etmek için Azure SQL veri ambarı'nda duraklatma işlem PowerShell kullanın. [İşlem devam](sql-data-warehouse-manage-compute-overview.md) veri ambarı kullanılmaya hazır olduğunuzda.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu öğretici için Azure PowerShell modülünün 5.1.1 veya daha sonraki bir sürümü gerekir. Şu anda kullandığınız sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/azurerm/install-azurerm-ps).

## <a name="before-you-begin"></a>Başlamadan önce

Bu hızlı başlangıçta, duraklatma ve sürdürme SQL data warehouse henüz varsayar. Bir oluşturmanız gerekiyorsa, kullanabileceğiniz [oluşturma ve bağlanma - portal](create-data-warehouse-portal.md) adlı bir veri ambarı oluşturmak için **mySampleDataWarehouse**.

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
2. Azure portalının sol taraftaki sayfasında **SQL veritabanları**’na tıklayın.
3. **SQL veritabanları** sayfasından **mySampleDataWarehouse** seçeneğini belirleyin. Veri ambarı açılır.

    ![Sunucu adı ve kaynak grubu](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Veritabanı adı olan veri ambarı adını yazın. Ayrıca sunucu adını ve kaynak grubunu da not alın. ,
5.  duraklatma ve sürdürme komutlarında bunları.
6. Sunucunuz foo.database.windows.net ise, PowerShell cmdlet'lerinde sunucu adı olarak yalnızca ilk bölümü kullanın. Önceki görüntüde tam sunucu adı newserver-20171113.database.windows.net şeklindedir. Sonek bırakın ve kullanmak **newserver-20171113** PowerShell cmdlet'inde sunucu adı olarak.

## <a name="pause-compute"></a>Duraklatma işlem
Maliyetlerden tasarruf etmek için duraklatma ve sürdürme işlem kaynaklarını isteğe bağlı. Örneğin, gece ve hafta sonları veritabanı kullanmıyorsanız, bu saatlerde duraklatabilir ve gün boyunca devam. Veritabanı duraklatılmış durumdayken işlem kaynakları için ücret alınmaz. Ancak, depolama için ücret ödemeye devam.

Bir veritabanı duraklatmak için kullanmak [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase) cmdlet'i. Aşağıdaki örnekte adlı bir veri ambarı duraklatır **mySampleDataWarehouse** adlı bir sunucuda barındırılan **newserver-20171113**. Adlı bir Azure kaynak grubunda sunucusudur **myResourceGroup**.


```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
```

Bir değişim bu sonraki örnekte veritabanı $database nesnesine alır. Ardından nesneye kanallar [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase). Sonuçlar nesne resultDatabase içinde depolanır. Son komut sonuçları gösterilmektedir.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```


## <a name="resume-compute"></a>İşlem devam et
Bir veritabanına başlamak için kullanmak [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase) cmdlet'i. Aşağıdaki örnek newserver-20171113 adlı bir sunucuda barındırılan mySampleDataWarehouse adlı bir veritabanı başlatır. MyResourceGroup adlı bir Azure kaynak grubunda sunucusudur.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" -DatabaseName "mySampleDataWarehouse"
```

Bir değişim bu sonraki örnekte veritabanı $database nesnesine alır. Ardından nesneye kanallar [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase) ve sonuçları $resultDatabase içinde depolar. Son komut sonuçları gösterilmektedir.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve veri ambarınızda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depoda tutmak istiyorsanız, duraklatabilirsiniz.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz.

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com)ve veri ambarınıza tıklayın.

    ![Kaynakları temizleme](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlemi sürdürmek için **Başlat**’a tıklayın.

2. İşlem veya depolama için ücretlendirilmemek üzere veri ambarını kaldırmak için **Sil**’e tıklayın.

3. Oluşturduğunuz SQL sunucusunu kaldırmak için tıklayın **mynewserver-20171113.database.windows.net**ve ardından **Sil**.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

4. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
Artık duraklatıldı ve veri ambarınıza yönelik işlem sürdürülüyor. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
