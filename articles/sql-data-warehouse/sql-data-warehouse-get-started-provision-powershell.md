---
title: "PowerShell kullanarak SQL Veri Ambarı oluşturma | Microsoft Belgeleri"
description: "PowerShell kullanarak SQL Data Warehouse oluşturma"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 97434863-7938-4129-8949-5a119f5949e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 3d13d4a0dd1d6e0b7361a57e167b06f0b717bfb4


---
# <a name="create-sql-data-warehouse-using-powershell"></a>PowerShell kullanarak SQL Data Warehouse oluşturma
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
> 
> 

Bu makalede PowerShell ile SQL Veri Ambarı oluşturma işlemi gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar
Başlamak için gerekli olanlar:

* **Azure hesabı**: Hesap oluşturmak için [Azure Ücretsiz Deneme][Azure Ücretsiz Deneme] veya [MSDN Azure Kredileri][MSDN Azure Kredileri] sayfasını ziyaret edin.
* **Azure SQL sunucusu**: Daha fazla ayrıntı için bkz. [Azure Portal ile Azure SQL Veritabanı mantıksal sunucusu oluşturma][Azure Portal ile Azure SQL Veritabanı mantıksal sunucusu oluşturma] veya [PowerShell ile Azure SQL Veritabanı mantıksal sunucusu oluşturma][PowerShell ile Azure SQL Veritabanı mantıksal sunucusu oluşturma].
* **Kaynak grubu**: Azure SQL sunucunuz ile aynı kaynak grubunu kullanın veya [nasıl kaynak grubu oluşturulacağına][nasıl kaynak grubu oluşturulacağına] bakın.
* **PowerShell 1.0.3 sürümü veya sonraki bir sürümü**: **Get-Module -ListAvailable -Name Azure** komutunu çalıştırarak sürümünüzü kontrol edebilirsiniz.  [Microsoft Web Platformu Yükleyicisi][Microsoft Web Platformu Yükleyicisi]'nden en son sürümü yükleyebilirsiniz.  En son sürümü yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma][Azure PowerShell'i yükleme ve yapılandırma].

> [!NOTE]
> Bir SQL Veri Ambarı'nın oluşturulması ek hizmet ücretlerinin alınmasına neden olabilir.  Fiyatlandırmayla ilgili ayrıntılı bilgi için bkz. [SQL Veri Ambarı fiyatlandırması][SQL Veri Ambarı fiyatlandırması].
> 
> 

## <a name="create-a-sql-data-warehouse"></a>SQL Data Warehouse oluşturma
1. Windows PowerShell'i açın.
2. Azure Resource Manager'da oturum açmak için bu cmdlet'i çalıştırın
   
    ```Powershell
    Login-AzureRmAccount
    ```
3. Geçerli oturumunuz için kullanmak istediğiniz aboneliği seçin.
   
    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. Veritabanı oluşturun. Bu örnekte "mywesteuroperesgp1" adlı kaynak grubunda bulunan "sqldwserver1" adlı sunucuda, hizmet hedefi düzeyi "DW400" olan "mynewsqldw" adlı bir veritabanı oluşturulmaktadır.
   
   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

Gerekli Parametreler şunlardır:

* **RequestedServiceObjectiveName**: İstediğiniz [DWU][DWU] sayısı.  Desteklenen değerler: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 ve DW6000.
* **DatabaseName**: Oluşturduğunuz SQL Data Warehouse'un adı.
* **ServerName**: Oluşturma işlemi için kullandığınız sunucunun adı (V12 olmalıdır).
* **ResourceGroupName**: Kullandığınız kaynak grubu.  Aboneliğinizdeki kullanılabilir kaynak gruplarını bulmak için Get-AzureResource komutunu kullanın.
* **Edition**: SQL Veri Ambarı oluşturmak için "DataWarehouse" olmalıdır.

İsteğe Bağlı Parametreler şunlardır:

* **CollationName**: Belirtilmezse varsayılan harmanlama SQL_Latin1_General_CP1_CI_AS şeklindedir.  Bir veritabanı üzerinde harmanlama değiştirilemez.
* **MaxSizeBytes**: Bir veritabanının varsayılan en büyük boyutu 10 GB'dir.

Parametre seçenekleri hakkında daha ayrıntılı bilgi için bkz. [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] ve [Veritabanı Oluşturma (Azure SQL Veri Ambarı)][Veritabanı Oluşturma (Azure SQL Veri Ambarı)].

## <a name="next-steps"></a>Sonraki adımlar
SQL Veri Ambarınız sağlandıktan sonra [örnek veri yüklemeyi][örnek veri yüklemeyi] deneyebilir ya da [geliştirme][geliştirme], [yükleme][yükleme] veya [geçirme][geçirme] işlemlerini nasıl gerçekleştirebileceğinizi inceleyebilirsiniz.

Programlama yoluyla SQL Veri Ambarı'nı yönetme hakkında daha fazla bilgi edinmek istiyorsanız [PowerShell cmdlet’leri ve REST API’lerinin][PowerShell cmdlet’leri ve REST API’lerinin] kullanımına ilişkin makalemize göz atın.

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[geçirme]: ./sql-data-warehouse-overview-migrate.md
[geliştirme]: ./sql-data-warehouse-overview-develop.md
[yükleme]: ./sql-data-warehouse-load-with-bcp.md
[örnek veri yükleme]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlet’leri ve REST API’leri]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[güvenlik duvarı kuralları]: ../sql-database-configure-firewall-settings.md

[Azure PowerShell’i yükleme ve yapılandırma]: ../powershell-install-configure.md
[Azure portalından SQL Veri Ambarı oluşturma]: ./sql-data-warehouse-get-started-provision.md
[Azure Portal ile Azure SQL Veritabanı mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[PowerShell ile Azure SQL Veritabanı mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[kaynak grubu oluşturma]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Veritabanı Oluşturma (Azure SQL Veri Ambarı)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platformu Yükleyicisi]: https://aka.ms/webpi-azps
[SQL Veri Ambarı fiyatlandırması]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Ücretsiz Deneme]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Kredileri]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Nov16_HO2-->


