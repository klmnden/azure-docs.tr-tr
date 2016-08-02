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
   ms.date="05/05/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Azure SQL Data Warehouse oluşturma

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Bu öğreticide Azure Portal'ı kullanarak AdventureWorksDW örnek veritabanı içeren bir SQL Data Warehouse oluşturacaksınız.


[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]


1. [Azure Portal](https://portal.azure.com)'da oturum açın.

2. **+ Yeni** > **Veri + Depolama** > **SQL Data Warehouse** seçeneğine tıklayın.

    ![Oluşturma](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. **SQL Data Warehouse** dikey penceresinde gerekli bilgileri girin ve "Oluştur" düğmesine basın.

    ![Veritabanı oluşturma](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Sunucu**: İlk önce sunucunuzu seçmenizi öneririz.  Var olan bir sunucuyu seçebilir veya [yeni bir sunucu oluşturabilirsiniz](./sql-data-warehouse-get-started-new-server.md). 

    - **Veritabanı adı**: SQL Data Warehouse'a başvurmak için kullanılacak olan ad.  Sunucu için benzersiz olmalıdır.
    
    - **Performans**: 400 DWU ile başlamanızı öneririz. Veri ambarınızın performansını ayarlamak için kaydırıcıyı sağa veya sola hareket ettirebilir ya da oluşturma işlemi tamamlandıktan sonra ölçeği artırıp azaltabilirsiniz.  DWU'lar hakkında daha fazla bilgi edinmek için [ölçeklendirme](./sql-data-warehouse-manage-compute-overview.md) ile ilgili belgelerimizi veya [fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/) sayfamızı inceleyebilirsiniz. 

    - **Abonelik**: Bu SQL Data Warehouse'un faturalanacağı aboneliği seçin.

    - **Kaynak grubu**: Kaynak grupları, Azure kaynak koleksiyonunu yönetmenize yardımcı olmak üzere tasarlanmış kapsayıcılardır. [Kaynak grupları](../azure-portal/resource-group-portal.md) hakkında daha fazla bilgi edinin.

    - **Kaynak seçme**: **Kaynak seç** > **Örnek** seçeneğine tıklayın. Şu anda yalnızca bir örnek veritabanı olduğundan, Örnek seçeneğini belirlediğinizde **Örnek seç** seçeneği Azure tarafından otomatik olarak AdventureWorksDW olarak doldurulur.

4. SQL Data Warehouse'unuzu oluşturmak için **Oluştur** düğmesine tıklayın.

5. SQL Data Warehouse'unuz birkaç dakika içinde hazır olur. İşlem tamamlandığında [Azure portalına](https://portal.azure.com) dönersiniz. SQL Data Warehouse'unuzu SQL Database'ler altında listelenmiş bir şekilde panonuzda veya SQL Data Warehouse'unuzu oluşturmak için kullandığınız kaynak grubunda bulabilirsiniz. 

    ![Portal görünümü](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL DataBase create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## Sonraki adımlar

Bir SQL Data Warehouse oluşturduğunuza göre [Bağlanın](./sql-data-warehouse-get-started-connect.md) ve sorgulamaya başlayın.

SQL Data Warehouse'a veri yüklemek için bkz. [yüklemeye genel bakış](./sql-data-warehouse-overview-load.md).

Var olan bir veritabanını SQL Data Warehouse'a geçirmeye çalışıyorsanız [Geçirmeye genel bakış](./sql-data-warehouse-overview-migrate.md) konu başlığına göz atın veya [Geçiş Yardımcı Programı](./sql-data-warehouse-migrate-migration-utility.md)'nı kullanın.




<!---HONumber=Jun16_HO2-->


