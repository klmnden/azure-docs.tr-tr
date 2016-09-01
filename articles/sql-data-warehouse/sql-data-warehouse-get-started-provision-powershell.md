<properties
   pageTitle="PowerShell kullanarak SQL Data Warehouse oluşturma | Microsoft Azure"
   description="PowerShell kullanarak SQL Data Warehouse oluşturma"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# PowerShell kullanarak SQL Data Warehouse oluşturma

> [AZURE.SELECTOR]
- [Azure Portalı](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Bu makalede PowerShell ile SQL Data Warehouse oluşturma işlemi gösterilmektedir.

## Ön koşullar

Başlamak için şunlar gereklidir:

- **Azure hesabı**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü][] veya [MSDN Azure Kredileri][] sayfasını ziyaret edin.
- **Azure SQL server**: Daha fazla bilgi için bkz. [Azure Portal ile Azure SQL Database mantıksal sunucusu oluşturma][] veya [PowerShell ile Azure SQL Database mantıksal sunucusu oluşturma][].
- **Kaynak grubu**: Azure SQL sunucunuz ile aynı kaynak grubunu kullanın veya [kaynak grubu oluşturma][] işlemine bakın.
- **PowerShell 1.0.3 sürümü veya sonraki bir sürümü**: **Get-Module -ListAvailable -Name Azure** komutunu çalıştırarak sürümünüzü kontrol edebilirsiniz.  [Microsoft Web Platformu Yükleyicisi][]'nden en son sürümü yükleyebilirsiniz.  En son sürümü yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma][].

> [AZURE.NOTE] Yeni bir SQL Data Warehouse'un oluşturulması ek hizmet ücretlerin alınmasına neden olabilir.  Fiyatlandırmayla ilgili ayrıntılı bilgi için bkz. [SQL Data Warehouse fiyatlandırması][].

## SQL Data Warehouse oluşturma

1. Windows PowerShell'i açın.
2. Azure Resource Manager'da oturum açmak için bu cmdlet'i çalıştırın

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Geçerli oturumunuz için kullanmak istediğiniz aboneliği seçin.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Veritabanı oluşturun. Bu örnekte "mywesteuroperesgp1" adlı kaynak grubunda bulunan "sqldwserver1" adlı sunucuda, hizmet hedefi düzeyi "DW400" olan "mynewsqldw" adlı yeni bir veritabanı oluşturulmaktadır.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse"
    ```

Bu cmdlet için gerekli parametreler şunlardır:

- **RequestedServiceObjectiveName**: İstediğiniz [DWU][] sayısı.  Desteklenen değerler: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 ve DW6000.
- **DatabaseName**: Oluşturduğunuz SQL Data Warehouse'un adı.
- **ServerName**: Oluşturma işlemi için kullandığınız sunucunun adı (V12 olmalıdır).
- **ResourceGroupName**: Kullandığınız kaynak grubu.  Aboneliğinizdeki kullanılabilir kaynak gruplarını bulmak için Get-AzureResource komutunu kullanın.
- **Edition**: SQL Data Warehouse oluşturmak için bu kısmı "DataWarehouse" olarak ayarlamanız gerekir.

Parametre seçenekleri hakkında daha ayrıntılı bilgi için bkz. [Veritabanı (Azure SQL Data Warehouse) Oluşturma][].
Komut başvurusu için bkz. [New-AzureRmSqlDatabase][].

## Sonraki adımlar

SQL Data Warehouse'unuzun hazırlanması tamamlandıktan sonra [örnek veri yükleme][] işlemini gerçekleştirmeyi veya [geliştirme][], [yükleme][] veya [geçirme][] işlemlerinin nasıl gerçekleştirileceğini incelemeyi tercih edebilirsiniz.

Programlama yoluyla SQL Data Warehouse'u yönetme hakkında daha fazla bilgi edinmek istiyorsanız [PowerShell cmdlet’leri ve REST API’lerinin][] kullanımına ilişkin makalemize göz atın.

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[geçirme]: ./sql-data-warehouse-overview-migrate.md
[geliştirme]: ./sql-data-warehouse-overview-develop.md
[yükleme]: ./sql-data-warehouse-load-with-bcp.md
[örnek veri yükleme]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlet’leri ve REST API’lerinin]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[güvenlik duvarı kuralları]: ../sql-database-configure-firewall-settings.md

[Azure PowerShell'i yükleme ve yapılandırma]: ../powershell/powershell-install-configure.md
[Azure Portal'dan SQL Data Warehouse oluşturma]: ./sql-data-warehouse-get-started-provision.md
[Azure Portal ile Azure SQL Database mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[PowerShell ile Azure SQL Database mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[kaynak grubu oluşturma]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Veritabanı (Azure SQL Data Warehouse) Oluşturma]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platformu Yükleyicisi]: https://aka.ms/webpi-azps
[SQL Data Warehouse fiyatlandırması]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Ücretsiz Deneme Sürümü]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Kredileri]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Aug16_HO4-->


