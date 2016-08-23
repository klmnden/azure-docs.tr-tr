<properties
   pageTitle="Azure Portal'da SQL Data Warehouse Oluşturma | Microsoft Azure"
   description="Azure Portal'da Azure SQL Data Warehouse oluşturmayı öğrenin"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/23/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# Azure SQL Data Warehouse oluşturma

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Bu öğreticide Azure Portal'ı kullanarak AdventureWorksDW örnek veritabanı içeren bir SQL Data Warehouse oluşturacaksınız.


## Ön koşullar

Başlamak için şunlar gereklidir:

- **Azure hesabı**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü][] veya [MSDN Azure Kredileri][] sayfasını ziyaret edin.
- **Azure SQL server**:  Daha fazla bilgi için bkz. [Azure Portal ile Azure SQL Database mantıksal sunucusu oluşturma][].

> [AZURE.NOTE] Yeni bir SQL Data Warehouse'un oluşturulması ek hizmet ücretlerin alınmasına neden olabilir.  Fiyatlandırmayla ilgili ayrıntılı bilgi için bkz. [SQL Data Warehouse fiyatlandırması][].

## SQL Data Warehouse oluşturma

1. [Azure Portal](https://portal.azure.com)'da oturum açın.

2. **+ Yeni** > **Veri + Depolama** > **SQL Data Warehouse** seçeneğine tıklayın.

    ![Oluşturma](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. **SQL Data Warehouse** dikey penceresinde gerekli bilgileri girin ve "Oluştur" düğmesine basın.

    ![Veritabanı oluşturma](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Sunucu**: İlk önce sunucunuzu seçmenizi öneririz.  Var olan bir sunucuyu seçebilir veya [yeni bir sunucu oluşturabilirsiniz](./sql-data-warehouse-get-started-new-server.md). 

    - **Veritabanı adı**: SQL Data Warehouse'a başvurmak için kullanılacak olan ad.  Sunucu için benzersiz olmalıdır.
    
    - **Performans**: 400 [DWU][DWU] ile başlamanız önerilir. Veri ambarınızın performansını ayarlamak için kaydırıcıyı sağa veya sola hareket ettirebilir ya da oluşturma işlemi tamamlandıktan sonra ölçeği artırıp azaltabilirsiniz.  DWU'lar hakkında daha fazla bilgi edinmek için [ölçeklendirme](./sql-data-warehouse-manage-compute-overview.md) ile ilgili belgelerimizi veya [fiyatlandırma][SQL Data Warehouse fiyatlandırması] sayfamızı inceleyebilirsiniz. 

    - **Abonelik**: Bu SQL Data Warehouse'un faturalanacağı [aboneliği] seçin.

    - **Kaynak grubu**: [Kaynak grupları][Kaynak grubu], Azure kaynak koleksiyonunu yönetmenize yardımcı olmak üzere tasarlanmış kapsayıcılardır. [Kaynak grupları](../resource-group-overview.md) hakkında daha fazla bilgi edinin.

    - **Kaynak seçme**: **Kaynak seç** > **Örnek** seçeneğine tıklayın. Şu anda yalnızca bir örnek veritabanı olduğundan, Örnek seçeneğini belirlediğinizde **Örnek seç** seçeneği Azure tarafından otomatik olarak AdventureWorksDW olarak doldurulur.

4. SQL Data Warehouse'unuzu oluşturmak için **Oluştur** düğmesine tıklayın.

5. SQL Data Warehouse'unuz birkaç dakika içinde hazır olur. İşlem tamamlandığında [Azure portalına](https://portal.azure.com) dönersiniz. SQL Data Warehouse'unuzu SQL Database'ler altında listelenmiş bir şekilde panonuzda veya SQL Data Warehouse'unuzu oluşturmak için kullandığınız kaynak grubunda bulabilirsiniz. 

    ![Portal görünümü](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL DataBase create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## Sonraki adımlar

Bir SQL Data Warehouse oluşturduğunuza göre [Bağlanın](./sql-data-warehouse-connect-overview.md) ve sorgulamaya başlayın.

SQL Data Warehouse'a veri yüklemek için bkz. [yüklemeye genel bakış](./sql-data-warehouse-overview-load.md).

Var olan bir veritabanını SQL Data Warehouse'a geçirmeye çalışıyorsanız [Geçirmeye genel bakış](./sql-data-warehouse-overview-migrate.md) konu başlığına göz atın veya [Geçiş Yardımcı Programı](./sql-data-warehouse-migrate-migration-utility.md)'nı kullanın.

Güvenlik duvarı kuralları, Transact-SQL kullanarak de yapılandırılabilir. Daha fazla bilgi için bkz. [sp_set_firewall_rule][] ve [sp_set_database_firewall_rule][].

[En iyi uygulamalar][] bölümüne bakmak da harika bir fikirdir.

<!--Article references-->
[Azure Portal ile Azure SQL Database mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[PowerShell ile Azure SQL Database mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[kaynak grupları]: ../resource-group-template-deploy-portal.md
[En iyi uygulamalar]: ./sql-data-warehouse-best-practices.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[aboneliği]: ../azure-glossary-cloud-terminology.md#subscription
[kaynak grubu]: ../azure-glossary-cloud-terminology.md#resource-group

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse fiyatlandırması]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Ücretsiz Deneme Sürümü]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Kredileri]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F




<!--HONumber=Aug16_HO1-->


