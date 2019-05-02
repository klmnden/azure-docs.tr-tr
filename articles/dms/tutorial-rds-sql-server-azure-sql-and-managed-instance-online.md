---
title: 'Öğretici: Çevrimiçi bir RDS SQL Server için Azure SQL veritabanı veya Azure SQL veritabanı yönetilen örneğine geçişi için Azure veritabanı geçiş hizmeti kullanın. | Microsoft Docs'
description: Azure SQL veritabanı yönetilen örneği Azure veritabanı geçiş hizmetini kullanarak veya bir çevrimiçi geçiş RDS SQL Server'dan Azure SQL veritabanı'na gerçekleştirmeyi öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 04/24/2019
ms.openlocfilehash: 75228098bcb62b83f8e93aebe600bffac62d6179
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64727345"
---
# <a name="tutorial-migrate-rds-sql-server-to-azure-sql-database-or-an-azure-sql-database-managed-instance-online-using-dms"></a>Öğretici: RDS SQL Server'ı Azure SQL veritabanı'na geçirme veya Azure SQL veritabanı yönetilen örneği çevrimiçi DMS kullanarak
RDS SQL Server örneğine veritabanlarını geçirmek için Azure veritabanı geçiş hizmetini kullanabilirsiniz [Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/) veya [Azure SQL veritabanı yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) en düşük kapalı kalma süresi. Bu öğreticide, geçiş **Adventureworks2012** geri yüklenen veritabanı bir RDS SQL Server örneği SQL Server 2012'in (veya üzeri) Azure SQL veritabanı veya Azure SQL veritabanı için Azure veritabanı geçişi kullanarak yönetilen örnek Hizmeti.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure SQL veritabanı örneği veya Azure SQL veritabanı yönetilen örneği oluşturun. 
> * Data Migration Yardımcısı'nı kullanarak örnek şemayı geçirme.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak geçiş projesi oluşturma.
> * Geçişi çalıştırma.
> * Geçişi izleme.
> * Geçiş raporu indirme.

> [!NOTE]
> Azure veritabanı geçiş hizmeti çevrimiçi bir geçiş gerçekleştirmek için Premium fiyatlandırma katmanını temel alan bir örneği oluşturmanız gerekir. Azure veritabanı geçiş hizmeti daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/database-migration/) sayfası.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, Azure Veritabanı Geçiş Hizmeti’nin bir örneğini hedef veritabanıyla aynı Azure bölgesinde oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, bir çevrimiçi geçiş RDS SQL Server'dan Azure SQL veritabanı veya bir Azure SQL veritabanı yönetilen örneği açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

- Oluşturma bir [RDS SQL Server veritabanı](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.SQLServer.html).
- Makalesinde ayrıntılı olarak izleyerek bunu Azure SQL veritabanı örneği oluşturma [Azure portalında bir Azure SQL veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).

    > [!NOTE]
    > Azure SQL veritabanı yönetilen örneğine geçiriyorsanız, ayrıntılı makaleyi izleyin [bir Azure SQL veritabanı yönetilen örnek oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started)ve ardından adlı boş bir veritabanı oluşturun **AdventureWorks2012**. 
 
- [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) 3.3 veya sonraki bir sürümünü indirip yükleyin.
- Bir Azure sanal ağ (VNET) için Azure veritabanı geçiş hizmeti Azure Resource Manager dağıtım modelini kullanarak oluşturun. Azure SQL veritabanı yönetilen örneğine geçiş yapıyorsanız, DMS örneği aynı sanal ağda Azure SQL veritabanı yönetilen örneği için kullanılan, ancak farklı bir alt ağ oluşturmak emin olun.  Alternatif olarak, farklı bir VNET için DMS kullanırsanız, VNET eşlemesi iki sanal ağ arasında oluşturmanız gerekir.
 
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
- Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesini sağlama amacıyla Azure SQL Veritabanı için sunucu düzeyinde [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını belirtin.
- Kaynak RDS SQL Server örneğine bağlanmak için kullanılan kimlik bilgileri "Processadmin" sunucu rolünün bir üyesi ve yükseltilecek olan tüm veritabanlarında "db_owner" veritabanı rollerinin bir üyesi olan bir hesapla ilişkili olduğundan emin olun.
- Yönetilen örnek bir Azure SQL veritabanı'na geçirme, hedef Azure SQL veritabanı örneğine bağlanmak için kullanılan kimlik bilgileri CONTROL DATABASE izninizin hedef Azure SQL veritabanlarına ve sysadmin rolünün bir üyesi olduğundan emin olun.
- Kaynak RDS SQL Server sürümünün SQL Server 2012 olmalıdır ve üstü. SQL Server örneğinizin sürümünü belirlemek için [SQL Server ve bileşenlerinin sürümünü ve güncelleştirme düzeyini belirleme](https://support.microsoft.com/help/321185/how-to-determine-the-version-edition-and-update-level-of-sql-server-an) başlıklı makaleye bakın.
- Değişiklik verilerini yakalama (CDC) RDS SQL Server veritabanı ve geçiş için seçilen tüm kullanıcı tablolarını etkinleştirin.
    > [!NOTE]
    > CDC bir RDS SQL Server veritabanında etkinleştirmek için aşağıdaki betiği kullanabilirsiniz.
    ```
    exec msdb.dbo.rds_cdc_enable_db 'AdventureWorks2012'
    ```
    > CDC tüm tablolarda etkinleştirmek için aşağıdaki betiği kullanabilirsiniz.
    ```
    use <Database name>
    go
    exec sys.sp_cdc_enable_table 
    @source_schema = N'Schema name', 
    @source_name = N'table name', 
    @role_name = NULL, 
    @supports_net_changes = 1 --for PK table 1, non PK tables 0
    GO
    ```
- Hedef Azure SQL Veritabanı'nda veritabanı tetikleyicilerini devre dışı bırakın.
    > [!NOTE]
    > Şu sorguyu kullanarak hedef Azure SQL Veritabanı'ndaki veritabanı tetikleyicilerini bulabilirsiniz:
    ```
    Use <Database name>
    select * from sys.triggers
    DISABLE TRIGGER (Transact-SQL)
    ```
    Daha fazla bilgi için [DISABLE TRIGGER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017) makalesine bakın.

## <a name="migrate-the-sample-schema"></a>Örnek şemayı geçirme
Azure SQL veritabanı'na geçiş şeması için DMA'yı kullanın.

> [!NOTE]
> DMA'da bir geçiş projesi oluşturmadan önce önkoşullarda belirtilen şekilde bir Azure SQL veritabanı sağladığınızdan emin olun. Bu öğreticinin amaçları doğrultusunda, Azure SQL veritabanı adı olarak kabul edilir **AdventureWorks2012**, ancak istediğiniz ad sağlayabilirsiniz.

**AdventureWorks2012** şemasını Azure SQL Veritabanına geçirmek için aşağıdaki adımları gerçekleştirin:

1.  Data Migration Yardımcısı'nda Yeni (+) simgesini ve ardından **Proje türü** bölümünde **Geçiş**'i seçin.
2.  Bir proje adı belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda da **Azure SQL Veritabanı**'nı seçin.
3.  **Geçiş Kapsamı** bölümünde **Yalnızca şema**'yı seçin.

    Yukarıdaki adımların gerçekleştirilmesinin ardından DMA arabiriminin aşağıda yer alan grafikteki gibi görünmesi gerekir:
    
    ![Data Migration Yardımcısı Projesi Oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-create-project.png)

4.  Projeyi oluşturmak için **Oluştur**'u seçin.
5.  DMA'da SQL Server'ınız için kaynak bağlantı ayrıntılarını belirtin, **Bağlan**'ı ve ardından **AdventureWorks2012** veritabanını seçin.

    ![Data Migration Yardımcısı Kaynak Bağlantı Ayrıntıları](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-source-connect.png)

6.  Seçin **sonraki**altında **hedef sunucuya Bağlan**, Azure SQL veritabanı için hedef bağlantı ayrıntılarını belirt, seçin **Connect**seçip **AdventureWorksAzure** Azure SQL veritabanı'nda önceden sağlanan veritabanı.

    ![Data Migration Yardımcısı Hedef Bağlantı Ayrıntıları](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-target-connect.png)

7.  **İleri**'yi seçerek **Nesneleri seçin** ekranına ilerleyin ve bu ekranda Azure SQL Veritabanına dağıtılacak **AdventureWorks2012** veritabanındaki şema nesnelerini belirtin.

    Varsayılan olarak, tüm nesneler seçilir.

    ![SQL Betiği Oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-assessment-source.png)

8.  **SQL betiği oluştur**'u seçerek SQL betiklerini oluşturun ve hata olup olmadığını inceleyin.

    ![Şema Betiği](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-schema-script.png)

9.  **Şemayı dağıtma**'yı seçerek şemayı Azure SQL Veritabanına dağıtın. Şema dağıtıldıktan sonra hedef sunucuda anomali olup olmadığını kontrol edin.

    ![Şemayı Dağıtma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme
1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.
 
   ![Portal aboneliklerini gösterme](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/portal-select-subscription1.png)
       
2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.
 
    ![Kaynak sağlayıcılarını gösterme](media/tutorial-sql-server-to-azure-sql-online/portal-select-resource-provider.png)
    
3.  "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.
 
    ![Kaynak sağlayıcısını kaydetme](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Örnek oluşturma
1.  Azure portalda +**Kaynak oluştur**'u seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/portal-marketplace.png)

2.  **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.
 
    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create1.png)
  
3.  **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz konumu seçin. 

5. Var olan bir sanal ağı (VNET) seçin veya yeni bir tane oluşturun.

    Sanal ağ, Azure Veritabanı Geçiş Hizmeti'nin kaynak SQL Server ve hedef Azure SQL Veritabanı örneğine erişmesini sağlar.

    Azure portalda sanal ağ oluşturma hakkında daha fazla bilgi için [Azure portalı kullanarak sanal ağ oluşturma](https://aka.ms/DMSVnet) makalesine bakın.

6. Bir fiyatlandırma katmanı seçin. Bu çevrimiçi geçiş için Premium fiyatlandırma Katmanı'ı seçtiğinizden emin olun.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

     ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-settings3.png)

7.  Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma
Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.
 
      ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.
 
     ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-instance-search.png)
 
3. +**Yeni Geçiş Projesi**'ni seçin.
4. Üzerinde **yeni geçiş projesi** projesi için bir ad belirtin, ekran **kaynak sunucu türü** metin kutusunda **SQL Server için AWS RDS**,  **Hedef sunucu türü** metin kutusunda **Azure SQL veritabanı**.

    > [!NOTE]
    > Hedef sunucu türü için **Azure SQL veritabanı** hem Azure SQL veritabanı tek veritabanı ve de Azure SQL veritabanı için farklı geçiş için yönetilen örneği.

5. İçinde **etkinlik türünü seçin** bölümünden **çevrimiçi veri geçişi**.

    > [!IMPORTANT]
    > Seçtiğinizden emin olun **çevrimiçi veri geçişi**; çevrimdışı geçişler, bu senaryo için desteklenmez.

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create-project4.png)

    > [!NOTE]
    > Alternatif olarak, seçebileceğiniz **yalnızca proje oluştur** geçiş projenizi oluşturmak ve daha sonra geçiş yürütmek için.

6. **Kaydet**’i seçin.

7. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

    ![Veritabanı Geçiş Hizmeti Etkinliği Oluşturma ve Çalıştırma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create-and-run-activity1.png)
 
## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme
1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server örneğinin bağlantı ayrıntılarını belirtin.
 
    Kaynak SQL Server örneği adı için Tam Etki Alanı Adı (FQDN) kullandığınızdan emin olun.

2. Kaynak sunucunuza güvenilir bir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Otomatik olarak imzalanan sertifika kullanarak şifrelenmiş SSL bağlantıları yüksek güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda otomatik olarak imzalanan sertifika ile SSL kullanımına güvenmemeniz gerekir.

   ![Kaynak Ayrıntıları](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-source-details3.png)

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme
1. **Kaydet**'i seçin ve **Geçiş hedef ayrıntıları** ekranında DMA kullanılarak **AdventureWorks2012** şemasının dağıtıldığı önceden sağlanmış Azure SQL Veritabanı olan hedef Azure SQL Veritabanı sunucusunun bağlantı ayrıntılarını belirtin.

    ![Hedef seçme](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-select-target3.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı, kaynak veritabanıyla aynı veritabanı adına sahipse Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-map-targets-activity4.png)

3. **Kaydet**'i seçin ve **Tablo seçme** ekranında tablo listesini genişletip etkilenen alan listesini inceleyin.

    Azure Veritabanı Geçiş Hizmeti hedef Azure SQL Veritabanı örneğinde bulunan tüm boş kaynak tablolarını seçer. Zaten verileri içeren tabloları yeniden geçirmek istiyorsanız, açıkça bu ekrandaki tablolar'ı seçmeniz gerekir.

    ![Tabloları seçme](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-configure-setting-activity4.png)

4.  Seçin **Kaydet**, aşağıdaki ayarlandıktan sonra **Gelişmiş çevrimiçi geçiş ayarları**.
    
    | Ayar | Açıklama |
    | ------------- | ------------- |
    | **En fazla paralel olarak yüklemek için tablo sayısı** | DMS, geçiş sırasında paralel olarak yürütülen tablo sayısını belirtir. Varsayılan değer 5'tir, ancak her POC geçişleri dayalı belirli bir geçiş gereksinimlerini karşılamak için en uygun bir değere da ayarlanabilir. |
    | **Zaman kaynak tablosu kesilmiş** | DMS, geçiş sırasında hedef tablosu keser olup olmadığını belirtir. Bu ayar, geçiş işleminin bir parçası bir veya daha fazla tablosu kesilmiş istediğinizde yararlı olabilir. |
    | **Büyük nesne (LOB) verileri ayarlarını yapılandır** | DMS sınırsız LOB verileri geçirir ya da belirli bir boyuta sınırları LOB veri geçişi belirtir.  Bir sınır yoktur, LOB veri geçişi, bu sınırı ötesinde herhangi bir LOB veri kesilmiş. Üretim geçişleri için seçilecek önerilir **LOB boyut izin** veri kaybını önlemek için. Sınırsız LOB boyutunun belirtirken seçin **LOB boyutu (KB) küçük olduğunda tek bir blok LOB geçirme verilerinde belirtilen** performansını artırmak için onay kutusunu işaretleyin. |
    
    ![Gelişmiş çevrimiçi geçiş ayarlarını belirleme](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-advanced-online-migration-settings.png)

5.  **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-migration-summary.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma
- **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi açılır ve etkinliğin **Durum** bilgisi **Başlatılıyor** olarak belirlenir.

    ![Etkinlik Durumu - başlatılıyor](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-activity-status2.png)

## <a name="monitor-the-migration"></a>Geçişi izleme
1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Çalıştırılıyor** olana kadar güncelleştirebilirsiniz.

2. **Tam veri yüklemesi** ve **Artımlı veri eşitleme** işlemleri için geçiş durumunu almak üzere belirli bir veritabanına tıklayın.

    ![Etkinlik Durumu - devam ediyor](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-activity-in-progress.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme
İlk Tam yük tamamlandıktan sonra, veritabanları **Geçiş için hazır** olarak işaretlenir.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

    ![Tam geçişi başlat](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-start-cutover.png)
 
2.  **Bekleyen değişiklikler** sayacı **0** değerini gösterene kadar bekleyerek kaynak veritabanına gelen tüm işlemleri durdurduğunuzdan emin olun.
3.  **Onayla**'yı ve ardından, **Uygula**'yı seçin.
4. Veritabanı geçişi durumu **Tamamlandı** olarak gösterildiğinde, uygulamalarınızı yeni hedef Azure SQL Veritabanı'na bağlayın.
 
    ![Etkinlik Durumu - tamamlandı](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-activity-completed.png)

## <a name="next-steps"></a>Sonraki adımlar
- Bilinen sorunlar ve Azure SQL DatabaseL çevrimiçi geçişi gerçekleştirirken sınırlamalar hakkında daha fazla bilgi için bkz [bilinen sorunlar ve geçici çözümler ile Azure SQL veritabanı çevrimiçi geçişlerini](known-issues-azure-sql-online.md).
- Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
- Azure SQL veritabanı hakkında daha fazla bilgi için bkz [Azure SQL veritabanı hizmeti nedir?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview).
- Yönetilen örnek, Azure SQL veritabanı hakkında bilgi için bu sayfaya bakın [Azure SQL veritabanı yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index).
