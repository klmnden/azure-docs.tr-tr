---
title: "Öğretici: SQL Server'dan Azure SQL veritabanı'nda tek ve havuza veritabanı çevrimiçi geçirmek için Azure veritabanı geçiş hizmeti kullanın. | Microsoft Docs"
description: Bir çevrimiçi geçiş SQL Server şirket içinden bir tek veritabanı veya havuza alınmış veritabanının Azure SQL veritabanı'nda Azure veritabanı geçiş hizmetini kullanarak gerçekleştirmek öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/01/2019
ms.openlocfilehash: 131b86fec5fb51c6ff6f29a8e0beed86145a24b7
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65136633"
---
# <a name="tutorial-migrate-sql-server-to-a-single-database-or-pooled-database-in-azure-sql-database-online-using-dms"></a>Öğretici: Tek veritabanı veya havuza alınmış veritabanını Azure SQL veritabanı'nda SQL Server'ı geçirme çevrimiçi DMS kullanarak

Azure Veritabanı Geçiş Hizmeti'ni kullanarak şirket içi SQL Server örneğindeki veritabanlarını minimum çalışmama süresi ile [Azure SQL Veritabanı](https://docs.microsoft.com/azure/sql-database/)'na geçirebilirsiniz. Bu öğreticide, geçiş **Adventureworks2012** SQL Server 2016 (veya üzeri) şirket içi örneğine geri yüklenen veritabanı tek veritabanı veya Azure veritabanı geçişi kullanarak havuza alınmış veritabanını Azure SQL veritabanı'nda Hizmeti.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> - Data Migration Yardımcısı'nı kullanarak şirket içi veritabanınızı değerlendirme.
> - Data Migration Yardımcısı'nı kullanarak örnek şemayı geçirme.
> - Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> - Azure Veritabanı Geçiş Hizmeti'ni kullanarak geçiş projesi oluşturma.
> - Geçişi çalıştırma.
> - Geçişi izleme.
> - Geçiş raporu indirme.

> [!NOTE]
> Azure veritabanı geçiş hizmeti çevrimiçi bir geçiş gerçekleştirmek için Premium fiyatlandırma katmanını temel alan bir örneği oluşturmanız gerekir. Azure veritabanı geçiş hizmeti daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/database-migration/) sayfası.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, Azure Veritabanı Geçiş Hizmeti’nin bir örneğini hedef veritabanıyla aynı Azure bölgesinde oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, SQL Server'dan bir çevrimiçi geçiş için bir tek veritabanı veya havuza alınmış veritabanının Azure SQL veritabanı'nda açıklanır. Çevrimdışı geçiş için bkz. [DMS kullanarak çevrimdışı biçimde SQL Server'ı Azure SQL Veritabanı’na geçirme](tutorial-sql-server-to-azure-sql.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

- [SQL Server 2012 veya sonraki bir sürümünü](https://www.microsoft.com/sql-server/sql-server-downloads) (herhangi bir sürüm) indirip yükleyin.
- [Sunucu Ağ Protokolünü Etkinleştirme veya Devre Dışı Bırakma](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) makalesindeki yönergeleri izleyerek SQL Server Express yüklemesi sırasında varsayılan olarak devre dışı bırakılan TCP/IP protokolünü etkinleştirin.
- Makalesinde ayrıntılı olarak izleyerek bunu Azure SQL veritabanı'nda tek (veya havuza alınmış) veritabanı oluşturma [Azure portalını kullanarak Azure SQL veritabanı tek veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-get-started).

    > [!NOTE]
    > SQL Server Integration Services (SSIS) kullanın ve Katalog veritabanı, SSIS projelerini/paketlerini (SSISDB) için SQL Server'dan Azure SQL veritabanı'na geçirmek istiyorsanız hedef SSISDB oluşturulur ve otomatik olarak sizin adınıza yönetilir olduğunda, SSIS Azure Data Factory (ADF) sağlayın. SSIS paketlerini geçirme hakkında daha fazla bilgi için bkz [geçirme SQL Server Integration Services paketlerini azure'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

- [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) 3.3 veya sonraki bir sürümünü indirip yükleyin.
- Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak bir Azure sanal ağ (VNET) için Azure veritabanı geçiş hizmeti oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

    > [!NOTE]
    > Microsoft Ağ eşlemesi ile ExpressRoute kullanıyorsanız, sanal ağ kurulumu sırasında şu Hizmet Ekle [uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) hangi hizmet sağlanacağı alt ağ için:
    > - Hedef veritabanı uç noktası (örneğin, SQL uç noktası, Cosmos DB uç noktası vb.)
    > - Depolama uç noktası
    > - Service bus uç noktası
    >
    > Azure veritabanı geçiş hizmeti internet bağlantısı olmadığı için bu gerekli bir yapılandırmadır.

- VNET ağ güvenlik grubu kurallarınızı aşağıdaki gelen iletişim bağlantı noktaları için Azure veritabanı geçiş hizmeti engelleme emin olun: 443, 53, 9354, 445, 12000. Azure VNET NSG trafiğini filtreleme hakkında ayrıntılı bilgi için [Ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) makalesine bakın.
- [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
- Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
- Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
- Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
- Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesini sağlama amacıyla Azure SQL Veritabanı için sunucu düzeyinde [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını belirtin.
- SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izinlerine sahip olduğundan emin olun.
- Hedef Azure SQL Veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL veritabanlarında CONTROL DATABASE iznine sahip olduğundan emin olun.
- Kaynak SQL Server, SQL Server 2005 veya sonraki bir sürümünde olmalıdır. SQL Server örneğinizin sürümünü belirlemek için [SQL Server ve bileşenlerinin sürümünü ve güncelleştirme düzeyini belirleme](https://support.microsoft.com/help/321185/how-to-determine-the-version-edition-and-update-level-of-sql-server-an) başlıklı makaleye bakın.
- Veritabanları Toplu günlük kurtarma veya Tam kurtarma modunda olmalıdır. SQL Server örneğiniz için yapılandırılan kurtarma modelini belirlemek için [Bir Veritabanının Kurtarma Modelini Görüntüleme veya Değiştirme (SQL Server)](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server?view=sql-server-2017) başlıklı makaleye bakın.
- Veritabanlarının Tam veritabanı yedeklerini aldığınızdan emin olun. Tam veritabanı yedeği oluşturmak için bkz [nasıl yapılır: Tam veritabanı yedekleme (Transact-SQL) oluşturma](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms191304(v=sql.105)).
- Tabloların hiçbirinde birincil anahtar bulunmuyorsa veritabanında ve belirli tablolarda Değişiklik Verilerini Yakalama (CDC) özelliğini etkinleştirin.
    > [!NOTE]
    > Birincil anahtarı bulunmayan tabloları bulmak için aşağıdaki betiği kullanabilirsiniz.

    ```sql
    USE <DBName>;
    go
    SELECT is_tracked_by_cdc, name AS TableName
    FROM sys.tables WHERE type = 'U' and is_ms_shipped = 0 AND
    OBJECTPROPERTY(OBJECT_ID, 'TableHasPrimaryKey') = 0;
     ```
    >Sonuçlarda "is_tracked_by_cdc" öğesinin "0"değerini aldığı bir veya daha fazla tablo yer alıyorsa [Enable and Disable Change Data Capture (SQL Server) (Değişiklik Verilerini Yakalama'yı Etkinleştirme ve Devre Dışı Bırakma (SQL Server))](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server?view=sql-server-2017) başlıklı makalede açıklanan işlemi kullanarak veritabanı ve belirli tablolar için değişiklik yakalama özelliğini etkinleştirin.

- Kaynak SQL Server için dağıtımcı rolünü yapılandırın.

    >[!NOTE]
    > Aşağıdaki sorguyu kullanarak çoğaltma bileşenlerinin yüklenip yüklenmediğini belirleyebilirsiniz.

    ```sql
    USE master;
    DECLARE @installed int;
    EXEC @installed = sys.sp_MS_replication_installed;
    SELECT @installed as installed;
    ```
    Sonuç, çoğaltma bileşenlerini yüklemeyi öneren bir hata iletisi döndürüyorsa [Install SQL Server replication (SQL Server çoğaltmasını yükleme )](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-replication?view=sql-server-2017) makalesindeki işlemi uygulayarak SQL Server çoğaltma bileşenlerini yükleyin.

    Çoğaltma zaten yüklüyse, aşağıdaki T-SQL komutunu kullanarak kaynak SQL Server'da dağıtım rolünün yapılandırılıp yapılandırılmadığını denetleyin.

    ```sql
    EXEC sp_get_distributor;
    ```

    Dağıtım ayarlanmadıysa, dağıtım sunucusunun yukarıdaki komut çıktısı için NULL değerini gösterdiği bölümde, [Dağıtımı Yapılandırma](https://docs.microsoft.com/sql/relational-databases/replication/configure-publishing-and-distribution?view=sql-server-2017) başlıklı makalede sunulan yönergeleri kullanarak dağıtımı yapılandırın.

- Hedef Azure SQL Veritabanı'nda veritabanı tetikleyicilerini devre dışı bırakın.
    >[!NOTE]
    > Şu sorguyu kullanarak hedef Azure SQL Veritabanı'ndaki veritabanı tetikleyicilerini bulabilirsiniz:

    ```sql
    Use <Database name>
    select * from sys.triggers
    DISABLE TRIGGER (Transact-SQL)
    ```
    Daha fazla bilgi için [DISABLE TRIGGER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017) makalesine bakın. 

## <a name="assess-your-on-premises-database"></a>Şirket içi veritabanınızı değerlendirme

Tek veritabanı veya havuza alınmış veritabanını Azure SQL veritabanı'nda bir şirket içi SQL Server örneğinden verileri geçirmeden önce geçişe engel olabilecek herhangi bir engelleyici soruna için SQL Server veritabanını değerlendirmek gerekir. Data Migration Yardımcısı 3.3 veya üzeri bir sürümü kullanarak [SQL Server geçiş değerlendirmesi yapma](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) makalesindeki adımları tamamlayın ve şirket içi veritabanı değerlendirmesi yapın.

Şirket içi bir veritabanını değerlendirmek için şu adımları izleyin:

1. DMA'da, Yeni (+) simgesini ve ardından, **Değerlendirme** proje türünü seçin.
2. Bir proje adı belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı** seçimini yapın ve ardından **Oluştur**'a tıklayarak projeyi oluşturun.

    Kaynak SQL Server veritabanını tek bir veritabanına geçirme veya havuza alınmış veritabanını Azure SQL veritabanı'nda değerlendirdiğiniz, rapor türleri birini veya ikisini de aşağıdaki değerlendirmesi seçebilirsiniz:

   - Veritabanı uyumluluğunu denetle
   - Özellik eşliğini denetle

     İki rapor türü de varsayılan olarak seçilidir.

3. DMA'daki **Seçenekler** ekranında, **Sonraki** seçeneğini belirleyin.
4. **Kaynak seçin** ekranının **Sunucuya bağlan** iletişim kutusunda SQL Server bağlantı bilgilerini girin ve **Bağlan**'ı seçin.
5. **Kaynak ekle** iletişim kutusunda **AdventureWorks2012**'yi, **Ekle**'yi ve ardından **Değerlendirmeyi Başlat**'ı seçin.

    > [!NOTE]
    > SSIS kullanırsanız, DMA SSISDB kaynağının değerlendirmesi şu anda desteklemiyor. Ancak, SSIS projelerini/paketlerini değerlendirilen/hedef Azure SQL veritabanı tarafından barındırılan SSISDB imzalanmasını olarak doğrulanması. SSIS paketlerini geçirme hakkında daha fazla bilgi için bkz [geçirme SQL Server Integration Services paketlerini azure'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

    Değerlendirme tamamlandığında aşağıdaki grafiğe benzer sonuçlar görüntülenir:

    ![Veri geçişi değerlendirmesi](media/tutorial-sql-server-to-azure-sql-online/dma-assessments.png)

    Tek veritabanları veya Azure SQL veritabanı'nda havuza alınmış veritabanları için Değerlendirmeler özellik eşlik ve geçiş için tek bir veritabanı dağıtma veya havuza alınmış veritabanının engelleme sorunlarını belirleyin.

    - **SQL Server özellik eşliği** kategorisi kapsamlı öneriler, Azure'daki alternatif yaklaşımlar ve geçiş projelerini planlama konusunda yardımcı olacak çıkarılabilecek adımlar sunar.
    - **Uyumluluk sorunları** kategorisi, şirket içi SQL Server veritabanlarının Azure SQL Veritabanına geçirilmesini engelleyebilecek uyumluluk sorunlarını yansıtan kısmen desteklenen ve desteklenmeyen özellikleri tanımlar. Bu sorunları gidermenize yardımcı olan öneriler de sağlanır.

6. İlgili seçenekleri belirleyerek değerlendirme sonuçlarındaki geçiş engelleyici sorunları ve özellik eşliği sorunlarını gözden geçirin.

## <a name="migrate-the-sample-schema"></a>Örnek şemayı geçirme

Sonra değerlendirmesiyle rahat ve seçili veritabanı tek veritabanı veya havuza alınmış Azure SQL veritabanı, veritabanı geçiş için uygun bir aday karşılandığında DMA şema Azure SQL veritabanı'na geçirmek için kullanın.

> [!NOTE]
> DMA'da bir geçiş projesi oluşturmadan önce önkoşullarda belirtilen şekilde bir Azure SQL veritabanı sağladığınızdan emin olun. Bu öğreticide Azure SQL Veritabanı’nın adının **AdventureWorksAzure** olduğu kabul edilmiştir, ancak istediğiniz adı kullanabilirsiniz.
> [!IMPORTANT]
> SSIS kullanırsanız, DMA şu anda kaynak SSISDB geçişini desteklemez, ancak, listelenen hedef Azure SQL veritabanı tarafından barındırılan SSISDB SSIS projelerini/paketlerini yeniden dağıtın. SSIS paketlerini geçirme hakkında daha fazla bilgi için bkz [geçirme SQL Server Integration Services paketlerini azure'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

Geçirilecek **AdventureWorks2012** bir tek veritabanı veya havuza alınmış veritabanının Azure SQL veritabanı için şema, aşağıdaki adımları gerçekleştirin:

1. Data Migration Yardımcısı'nda Yeni (+) simgesini ve ardından **Proje türü** bölümünde **Geçiş**'i seçin.
2. Bir proje adı belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda da **Azure SQL Veritabanı**'nı seçin.
3. **Geçiş Kapsamı** bölümünde **Yalnızca şema**'yı seçin.

    Yukarıdaki adımların gerçekleştirilmesinin ardından DMA arabiriminin aşağıda yer alan grafikteki gibi görünmesi gerekir:

    ![Data Migration Yardımcısı Projesi Oluşturma](media/tutorial-sql-server-to-azure-sql-online/dma-create-project.png)

4. Projeyi oluşturmak için **Oluştur**'u seçin.
5. DMA'da SQL Server'ınız için kaynak bağlantı ayrıntılarını belirtin, **Bağlan**'ı ve ardından **AdventureWorks2012** veritabanını seçin.

    ![Data Migration Yardımcısı Kaynak Bağlantı Ayrıntıları](media/tutorial-sql-server-to-azure-sql-online/dma-source-connect.png)

6. **Hedef sunucuya bağlan** bölümünde **Sonraki** seçeneğini belirleyin, Azure SQL veritabanı için hedef bağlantı ayrıntılarını belirtin, **Bağlan**'ı ve ardından, Azure SQL Veritabanı'nda sağlamış olduğunuz **AdventureWorksAzure** veritabanını seçin.

    ![Data Migration Yardımcısı Hedef Bağlantı Ayrıntıları](media/tutorial-sql-server-to-azure-sql-online/dma-target-connect.png)

7. **İleri**'yi seçerek **Nesneleri seçin** ekranına ilerleyin ve bu ekranda Azure SQL Veritabanına dağıtılacak **AdventureWorks2012** veritabanındaki şema nesnelerini belirtin.

    Varsayılan olarak, tüm nesneler seçilir.

    ![SQL Betiği Oluşturma](media/tutorial-sql-server-to-azure-sql-online/dma-assessment-source.png)

8. **SQL betiği oluştur**'u seçerek SQL betiklerini oluşturun ve hata olup olmadığını inceleyin.

    ![Şema Betiği](media/tutorial-sql-server-to-azure-sql-online/dma-schema-script.png)

9. **Şemayı dağıtma**'yı seçerek şemayı Azure SQL Veritabanına dağıtın. Şema dağıtıldıktan sonra hedef sunucuda anomali olup olmadığını kontrol edin.

    ![Şemayı Dağıtma](media/tutorial-sql-server-to-azure-sql-online/dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-sql-server-to-azure-sql-online/portal-select-subscription1.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-sql-server-to-azure-sql-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-sql-server-to-azure-sql-online/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Örnek oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-sql-server-to-azure-sql-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-sql-server-to-azure-sql-online/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz konumu seçin. 

5. Var olan bir sanal ağı (VNET) seçin veya yeni bir tane oluşturun.

    Sanal ağ, Azure Veritabanı Geçiş Hizmeti'nin kaynak SQL Server ve hedef Azure SQL Veritabanı örneğine erişmesini sağlar.

    Azure portalda sanal ağ oluşturma hakkında daha fazla bilgi için [Azure portalı kullanarak sanal ağ oluşturma](https://aka.ms/DMSVnet) makalesine bakın.

6. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-sql-server-to-azure-sql-online/dms-settings2.png)

7. select **Oluştur** hizmeti oluşturmak için.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-sql-server-to-azure-sql-online/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-sql-server-to-azure-sql-online/dms-instance-search.png)

3. +**Yeni Geçiş Projesi**'ni seçin.
4. **Yeni geçiş projesi** ekranında proje için bir ad belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda ise **Azure SQL Veritabanı** seçeneğini belirleyin.
5. İçinde **etkinlik türünü seçin** bölümünden **çevrimiçi veri geçişi**.

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-sql-server-to-azure-sql-online/dms-create-project3.png)

    > [!NOTE]
    > Alternatif olarak, seçebileceğiniz **yalnızca proje oluştur** geçiş projenizi oluşturmak ve daha sonra geçiş yürütmek için.

6. **Kaydet**’i seçin.

7. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

    ![Veritabanı Geçiş Hizmeti Etkinliği Oluşturma ve Çalıştırma](media/tutorial-sql-server-to-azure-sql-online/dms-create-and-run-activity.png)

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server örneğinin bağlantı ayrıntılarını belirtin.

    Kaynak SQL Server örneği adı için Tam Etki Alanı Adı (FQDN) kullandığınızdan emin olun. DNS ad çözümlemenin mümkün olmadığı durumlarda IP Adresini de kullanabilirsiniz.

2. Kaynak sunucunuza güvenilir bir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Otomatik olarak imzalanan sertifika kullanarak şifrelenmiş SSL bağlantıları yüksek güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda otomatik olarak imzalanan sertifika ile SSL kullanımına güvenmemeniz gerekir.

   ![Kaynak Ayrıntıları](media/tutorial-sql-server-to-azure-sql-online/dms-source-details3.png)

    > [!IMPORTANT]
    > SSIS kullanırsanız, DMS kaynak SSISDB geçişi şu anda desteklememektedir ancak, listelenen hedef Azure SQL veritabanı tarafından barındırılan SSISDB SSIS projelerini/paketlerini yeniden dağıtın. SSIS paketlerini geçirme hakkında daha fazla bilgi için bkz [geçirme SQL Server Integration Services paketlerini azure'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. **Kaydet**'i seçin ve **Geçiş hedef ayrıntıları** ekranında DMA kullanılarak **AdventureWorks2012** şemasının dağıtıldığı önceden sağlanmış Azure SQL Veritabanı olan hedef Azure SQL Veritabanı sunucusunun bağlantı ayrıntılarını belirtin.

    ![Hedef seçme](media/tutorial-sql-server-to-azure-sql-online/dms-select-target3.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı, kaynak veritabanıyla aynı veritabanı adına sahipse Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-sql-server-to-azure-sql-online/dms-map-targets-activity3.png)

3. **Kaydet**'i seçin ve **Tablo seçme** ekranında tablo listesini genişletip etkilenen alan listesini inceleyin.

    Azure Veritabanı Geçiş Hizmeti hedef Azure SQL Veritabanı örneğinde bulunan tüm boş kaynak tablolarını seçer. Veri içeren tabloları yeniden geçirmek isterseniz bu dikey pencerede tabloları seçmeniz gerekir.

    ![Tabloları seçme](media/tutorial-sql-server-to-azure-sql-online/dms-configure-setting-activity3.png)

4. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-sql-server-to-azure-sql-online/dms-migration-summary.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

- **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi açılır ve etkinliğin **Durum** bilgisi **Başlatılıyor** olarak belirlenir.

    ![Etkinlik Durumu - başlatılıyor](media/tutorial-sql-server-to-azure-sql-online/dms-activity-status2.png)

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Çalıştırılıyor** olana kadar güncelleştirebilirsiniz.

2. **Tam veri yüklemesi** ve **Artımlı veri eşitleme** işlemleri için geçiş durumunu almak üzere belirli bir veritabanına tıklayın.

    ![Etkinlik Durumu - devam ediyor](media/tutorial-sql-server-to-azure-sql-online/dms-activity-in-progress.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk Tam yük tamamlandıktan sonra, veritabanları **Geçiş için hazır** olarak işaretlenir.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

    ![Tam geçişi başlat](media/tutorial-sql-server-to-azure-sql-online/dms-start-cutover.png)

2. **Bekleyen değişiklikler** sayacı **0** değerini gösterene kadar bekleyerek kaynak veritabanına gelen tüm işlemleri durdurduğunuzdan emin olun.
3. **Onayla**'yı ve ardından, **Uygula**'yı seçin.
4. Veritabanı geçişi durumu **Tamamlandı** olarak gösterildiğinde, uygulamalarınızı yeni hedef Azure SQL Veritabanı'na bağlayın.

    ![Etkinlik Durumu - tamamlandı](media/tutorial-sql-server-to-azure-sql-online/dms-activity-completed.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veritabanı Geçiş Hizmetini (DMS) kullanarak SQL geçişi yapma](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=3b671509-c3cd-4495-8e8f-354acfa09587) uygulamalı laboratuvarı.
- Bilinen sorunlar ve Azure SQL veritabanı çevrimiçi geçiş gerçekleştirirken sınırlamalar hakkında daha fazla bilgi için bkz [bilinen sorunlar ve geçici çözümler ile Azure SQL veritabanı çevrimiçi geçişlerini](known-issues-azure-sql-online.md).
- Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
- Azure SQL veritabanı hakkında daha fazla bilgi için bkz [Azure SQL veritabanı hizmeti nedir?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview).
