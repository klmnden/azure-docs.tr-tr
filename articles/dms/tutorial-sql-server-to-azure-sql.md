---
title: "Öğretici: SQL Server'dan Azure SQL veritabanı'nda bir tek ve havuza veritabanı çevrimdışı geçirmek için Azure veritabanı geçiş hizmeti kullanın. | Microsoft Docs"
description: Azure veritabanı geçiş Hizmeti'ni kullanarak tek veritabanı veya havuza alınmış Azure SQL veritabanını çevrimdışına veritabanında SQL Server şirket içinden geçirmeyi öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 07/09/2019
ms.openlocfilehash: c4d2e70d0f200f3a31948cfaa43def8e1b699878
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706880"
---
# <a name="tutorial-migrate-sql-server-to-a-single-database-or-pooled-database-in-azure-sql-database-offline-using-dms"></a>Öğretici: Tek veritabanı veya havuza alınmış veritabanını Azure SQL veritabanı'nda SQL Server'ı geçirme DMS kullanarak çevrimdışı

Azure Veritabanı Geçiş Hizmeti'ni kullanarak şirket içi SQL Server örneğindeki veritabanlarını [Azure SQL Veritabanına](https://docs.microsoft.com/azure/sql-database/) geçirebilirsiniz. Bu öğreticide, geçiş **Adventureworks2012** SQL Server 2016 (veya üzeri) şirket içi örneğine geri yüklenen veritabanı tek veritabanı veya Azure veritabanı geçişi kullanarak havuza alınmış veritabanını Azure SQL veritabanı'nda Hizmeti.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
>
> - Data Migration Yardımcısı'nı kullanarak şirket içi veritabanınızı değerlendirme.
> - Data Migration Yardımcısı'nı kullanarak örnek şemayı geçirme.
> - Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> - Azure Veritabanı Geçiş Hizmeti'ni kullanarak geçiş projesi oluşturma.
> - Geçişi çalıştırma.
> - Geçişi izleme.
> - Geçiş raporu indirme.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, SQL Server'dan çevrimdışı bir geçiş için bir tek veritabanı veya havuza alınmış veritabanının Azure SQL veritabanı'nda açıklanır. Çevrimiçi geçiş için bkz. [DMS kullanarak çevrimiçi biçimde SQL Server'ı Azure SQL Veritabanı’na geçirme](tutorial-sql-server-azure-sql-online.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

- İndirme ve yükleme [SQL Server 2016 veya sonraki](https://www.microsoft.com/sql-server/sql-server-downloads).
- [Sunucu Ağ Protokolünü Etkinleştirme veya Devre Dışı Bırakma](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) makalesindeki yönergeleri izleyerek SQL Server Express yüklemesi sırasında varsayılan olarak devre dışı bırakılan TCP/IP protokolünü etkinleştirin.
- Makalesinde ayrıntılı olarak izleyerek bunu Azure SQL veritabanı'nda tek (veya havuza alınmış) veritabanı oluşturma [Azure portalını kullanarak Azure SQL veritabanı tek veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-get-started).

    > [!NOTE]
    > SQL Server Integration Services (SSIS) kullanın ve Katalog veritabanı, SSIS projelerini/paketlerini (SSISDB) için SQL Server'dan Azure SQL veritabanı'na geçirmek istiyorsanız hedef SSISDB oluşturulur ve otomatik olarak sizin adınıza yönetilir olduğunda, SSIS Azure Data Factory (ADF) sağlayın. SSIS paketlerini geçirme hakkında daha fazla bilgi için bkz [geçirme SQL Server Integration Services paketlerini azure'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).
  
- [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) 3.3 veya üzeri sürümünü indirip yükleyin.
- Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak bir Azure sanal ağı (VNet) için Azure veritabanı geçiş hizmeti oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network/)ve özellikle hızlı başlangıç makalelerini ile adım adım ayrıntıları.

    > [!NOTE]
    > Microsoft Ağ eşlemesi ile ExpressRoute kullanıyorsanız, sanal ağ kurulumu sırasında şu Hizmet Ekle [uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) hangi hizmet sağlanacağı alt ağ için:
    > - Hedef veritabanı uç noktası (örneğin, SQL uç noktası, Cosmos DB uç noktası vb.)
    > - Depolama uç noktası
    > - Service bus uç noktası
    >
    > Azure veritabanı geçiş hizmeti internet bağlantısı olmadığı için bu gerekli bir yapılandırmadır.

- VNet Ağ Güvenlik Grubu kurallarının aşağıdaki Azure Veritabanı Geçiş Hizmeti’ne gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445, 12000. Azure VNet NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
- Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
- Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
- Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
- Sunucu düzeyinde IP oluşturma [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) için Azure veritabanı geçiş hizmeti hedef veritabanlarına erişim izni vermek Azure SQL veritabanı sunucusu. Azure veritabanı geçiş hizmeti için kullanılan sanal ağ alt ağ aralığını belirtin.
- SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izinlerine sahip olduğundan emin olun.
- Hedef Azure SQL Veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL veritabanlarında CONTROL DATABASE iznine sahip olduğundan emin olun.

## <a name="assess-your-on-premises-database"></a>Şirket içi veritabanınızı değerlendirme

Tek veritabanı veya havuza alınmış veritabanını Azure SQL veritabanı'nda bir şirket içi SQL Server örneğinden verileri geçirmeden önce geçişe engel olabilecek herhangi bir engelleyici soruna için SQL Server veritabanını değerlendirmek gerekir. Data Migration Yardımcısı 3.3 veya üzeri bir sürümü kullanarak [SQL Server geçiş değerlendirmesi yapma](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) makalesindeki adımları tamamlayın ve şirket içi veritabanı değerlendirmesi yapın. Yapılması gereken adımların özeti aşağıda verilmiştir:

1. Data Migration Yardımcısı'nda Yeni (+) simgesini ve **Değerlendirme** proje türünü seçin.
2. Bir proje adı belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı** seçimini yapın ve ardından **Oluştur**'a tıklayarak projeyi oluşturun.

    Kaynak SQL Server veritabanını tek bir veritabanına geçirme veya havuza alınmış veritabanını Azure SQL veritabanı'nda değerlendirdiğiniz, rapor türleri birini veya ikisini de aşağıdaki değerlendirmesi seçebilirsiniz:

   - Veritabanı uyumluluğunu denetle
   - Özellik eşliğini denetle

    İki rapor türü de varsayılan olarak seçilidir.

3. Data Migration Yardımcısı'nın **Seçenekler** ekranında **İleri**'yi seçin.
4. **Kaynak seçin** ekranının **Sunucuya bağlan** iletişim kutusunda SQL Server bağlantı bilgilerini girin ve **Bağlan**'ı seçin.
5. **Kaynak ekle** iletişim kutusunda **AdventureWorks2012**'yi, **Ekle**'yi ve ardından **Değerlendirmeyi Başlat**'ı seçin.

    > [!NOTE]
    > SSIS kullanırsanız, DMA SSISDB kaynağının değerlendirmesi şu anda desteklemiyor. Ancak, SSIS projelerini/paketlerini değerlendirilen/hedef Azure SQL veritabanı tarafından barındırılan SSISDB imzalanmasını olarak doğrulanması. SSIS paketlerini geçirme hakkında daha fazla bilgi için bkz [geçirme SQL Server Integration Services paketlerini azure'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

    Değerlendirme tamamlandığında aşağıdaki grafiğe benzer sonuçlar görüntülenir:

    ![Veri geçişi değerlendirmesi](media/tutorial-sql-server-to-azure-sql/dma-assessments.png)

    Tek veritabanları veya Azure SQL veritabanı'nda havuza alınmış veritabanları için Değerlendirmeler özellik eşlik ve geçiş için tek bir veritabanı dağıtma veya havuza alınmış veritabanının engelleme sorunlarını belirleyin.

    - **SQL Server özellik eşliği** kategorisi kapsamlı öneriler, Azure'daki alternatif yaklaşımlar ve geçiş projelerini planlama konusunda yardımcı olacak çıkarılabilecek adımlar sunar.
    - **Uyumluluk sorunları** kategorisi, şirket içi SQL Server veritabanlarının Azure SQL Veritabanına geçirilmesini engelleyebilecek uyumluluk sorunlarını yansıtan kısmen desteklenen ve desteklenmeyen özellikleri tanımlar. Bu sorunları gidermenize yardımcı olan öneriler de sağlanır.

6. İlgili seçenekleri belirleyerek değerlendirme sonuçlarındaki geçiş engelleyici sorunları ve özellik eşliği sorunlarını gözden geçirin.

## <a name="migrate-the-sample-schema"></a>Örnek şemayı geçirme

Sonra değerlendirmesiyle rahat ve seçili veritabanı tek veritabanı veya havuza alınmış Azure SQL veritabanı, veritabanı geçiş için uygun bir aday karşılandığında DMA şema Azure SQL veritabanı'na geçirmek için kullanın.

> [!NOTE]
> Data Migration Yardımcısı'nda bir geçiş projesi oluşturmadan önce önkoşullarda belirtilen şekilde bir Azure SQL veritabanı sağladığınızdan emin olun. Bu öğreticide Azure SQL Veritabanı’nın adının **AdventureWorksAzure** olduğu kabul edilmiştir, ancak istediğiniz adı kullanabilirsiniz.
> [!IMPORTANT]
> SSIS kullanırsanız, DMA şu anda kaynak SSISDB geçişini desteklemez, ancak, listelenen hedef Azure SQL veritabanı tarafından barındırılan SSISDB SSIS projelerini/paketlerini yeniden dağıtın. SSIS paketlerini geçirme hakkında daha fazla bilgi için bkz [geçirme SQL Server Integration Services paketlerini azure'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

Geçirilecek **AdventureWorks2012** bir tek veritabanı veya havuza alınmış veritabanının Azure SQL veritabanı için şema, aşağıdaki adımları gerçekleştirin:

1. Data Migration Yardımcısı'nda Yeni (+) simgesini ve ardından **Proje türü** bölümünde **Geçiş**'i seçin.
2. Bir proje adı belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda da **Azure SQL Veritabanı**'nı seçin.
3. **Geçiş Kapsamı** bölümünde **Yalnızca şema**'yı seçin.

    Yukarıdaki adımları gerçekleştirdikten sonra aşağıdaki görüntüde gösterildiği gibi Data Migration Yardımcısı arabiriminin görünmesi gerekir:

    ![Data Migration Yardımcısı Projesi Oluşturma](media/tutorial-sql-server-to-azure-sql/dma-create-project.png)

4. Projeyi oluşturmak için **Oluştur**'u seçin.
5. Data Migration Yardımcısı'nda kaynak SQL Server için bağlantı bilgilerini belirtin, **Bağlan**'ı ve ardından **AdventureWorks2012** veritabanını seçin.

    ![Data Migration Yardımcısı Kaynak Bağlantı Ayrıntıları](media/tutorial-sql-server-to-azure-sql/dma-source-connect.png)

6. **Hedef sunucuya bağlan** bölümünde **İleri**'yi seçin, Azure SQL veritabanı için hedef bağlantı ayrıntılarını belirtin, **Bağlan**'ı ve ardından Azure SQL veritabanında sağladığınız **AdventureWorksAzure** veritabanını seçin.

    ![Data Migration Yardımcısı Hedef Bağlantı Ayrıntıları](media/tutorial-sql-server-to-azure-sql/dma-target-connect.png)

7. **İleri**'yi seçerek **Nesneleri seçin** ekranına ilerleyin ve bu ekranda Azure SQL Veritabanına dağıtılacak **AdventureWorks2012** veritabanındaki şema nesnelerini belirtin.

    Varsayılan olarak, tüm nesneler seçilir.

    ![SQL Betiği Oluşturma](media/tutorial-sql-server-to-azure-sql/dma-assessment-source.png)

8. **SQL betiği oluştur**'u seçerek SQL betiklerini oluşturun ve hata olup olmadığını inceleyin.

    ![Şema Betiği](media/tutorial-sql-server-to-azure-sql/dma-schema-script.png)

9. **Şemayı dağıtma**'yı seçerek şemayı Azure SQL Veritabanına dağıtın. Şema dağıtıldıktan sonra hedef sunucuda anomali olup olmadığını kontrol edin.

    ![Şemayı Dağıtma](media/tutorial-sql-server-to-azure-sql/dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-sql-server-to-azure-sql/portal-select-subscription1.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-sql-server-to-azure-sql/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-sql-server-to-azure-sql/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Örnek oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-sql-server-to-azure-sql/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-sql-server-to-azure-sql/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz konumu seçin.

5. Mevcut bir VNet seçin veya yeni bir tane oluşturun.

    Sanal ağ, erişim kaynak SQL Server için Azure veritabanı geçiş hizmeti ve hedef Azure SQL veritabanı örneği sağlar.

    Azure portalında VNet oluşturma hakkında daha fazla bilgi için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

6. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-sql-server-to-azure-sql/dms-settings2.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

     ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-sql-server-to-azure-sql/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-sql-server-to-azure-sql/dms-instance-search.png)

3. +**Yeni Geçiş Projesi**'ni seçin.
4. **Yeni geçiş projesi** ekranında proje için bir ad belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı** ve **Etkinlik türünü seçin** alanında **Çevrimdışı veri geçişi** seçimini yapın. 

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-sql-server-to-azure-sql/dms-create-project2.png)

5. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server örneğinin bağlantı ayrıntılarını belirtin.

    Kaynak SQL Server örneği adı için Tam Etki Alanı Adı (FQDN) kullandığınızdan emin olun. DNS ad çözümlemenin mümkün olmadığı durumlarda IP Adresini de kullanabilirsiniz.

2. Kaynak sunucunuza bir güvenilir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Otomatik olarak imzalanan sertifika kullanarak şifrelenmiş SSL bağlantıları yüksek güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda otomatik olarak imzalanan sertifika ile SSL kullanımına güvenmemeniz gerekir.

   ![Kaynak Ayrıntıları](media/tutorial-sql-server-to-azure-sql/dms-source-details2.png)

    > [!IMPORTANT]
    > SSIS kullanırsanız, DMS kaynak SSISDB geçişi şu anda desteklememektedir ancak, listelenen hedef Azure SQL veritabanı tarafından barındırılan SSISDB SSIS projelerini/paketlerini yeniden dağıtın. SSIS paketlerini geçirme hakkında daha fazla bilgi için bkz [geçirme SQL Server Integration Services paketlerini azure'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. **Kaydet**'i seçin ve **Geçiş hedef ayrıntıları** ekranında Data Migration Yardımcısı kullanılarak **AdventureWorks2012** şemasının dağıtıldığı önceden sağlanmış Azure SQL Veritabanı olan hedef Azure SQL Veritabanı sunucusunun bağlantı ayrıntılarını girin.

    ![Hedef seçme](media/tutorial-sql-server-to-azure-sql/dms-select-target2.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı, kaynak veritabanıyla aynı veritabanı adına sahipse Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-sql-server-to-azure-sql/dms-map-targets-activity2.png)

3. **Kaydet**'i seçin ve **Tablo seçme** ekranında tablo listesini genişletip etkilenen alan listesini inceleyin.

    Azure Veritabanı Geçiş Hizmeti'nin hedef Azure SQL Veritabanı örneğinde bulunan tüm boş kaynak tablolarını seçtiğine dikkat edin. Veri içeren tabloları yeniden geçirmek isterseniz bu dikey pencerede tabloları seçmeniz gerekir.

    ![Tabloları seçme](media/tutorial-sql-server-to-azure-sql/dms-configure-setting-activity2.png)

4. **Kaydet**'i seçin ve **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

5. **Doğrulama seçeneği** bölümünü genişleterek **Doğrulama seçeneğini belirleyin** ekranını açın ve geçirilen veritabanlarında **Şema karşılaştırması**, **Veri tutarlılığı** ve **Sorgu doğruluğu** doğrulaması yapma tercihlerini belirtin.

    ![Doğrulama seçeneğini belirleme](media/tutorial-sql-server-to-azure-sql/dms-configuration2.png)

6. **Kaydet**'i seçin ve özeti gözden geçirerek kaynak ve hedef ayrıntılarının belirttiğiniz verilere uyduğundan emin olun.

    ![Geçiş Özeti](media/tutorial-sql-server-to-azure-sql/dms-run-migration2.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

- **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi açılır ve etkinliğin **Durum** bilgisi **Beklemede** olarak değişir.

    ![Etkinlik Durumu](media/tutorial-sql-server-to-azure-sql/dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek geçişin **Durum** bilgisi **Tamamlandı** olana kadar gösterilen verileri güncelleştirebilirsiniz.

    ![Etkinlik Durumu Tamamlandı](media/tutorial-sql-server-to-azure-sql/dms-completed-activity1.png)

2. Geçiş tamamlandıktan sonra **Raporu indir**'i seçerek geçiş işleminin ayrıntılarını içeren raporu indirebilirsiniz.

3. Azure SQL Veritabanı sunucusundaki hedef veritabanlarını doğrulayın.

### <a name="additional-resources"></a>Ek kaynaklar

- [Azure Veritabanı Geçiş Hizmetini (DMS) kullanarak SQL geçişi yapma](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=3b671509-c3cd-4495-8e8f-354acfa09587) uygulamalı laboratuvarı.
- Bilinen sorunlar ve Azure SQL veritabanı çevrimiçi geçiş gerçekleştirirken sınırlamalar hakkında daha fazla bilgi için bkz [bilinen sorunlar ve geçici çözümler ile Azure SQL veritabanı çevrimiçi geçişlerini](known-issues-azure-sql-online.md).
- Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
- Azure SQL veritabanı hakkında daha fazla bilgi için bkz [Azure SQL veritabanı hizmeti nedir?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview).
