---
title: İzleme günlük analizi ile Azure SQL veri eşitleme | Microsoft Docs
description: Günlük analizi kullanarak Azure SQL veri eşitleme izleneceği hakkında bilgi edinin
services: sql-database
ms.date: 04/01/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
manager: craigg
ms.custom: data-sync
ms.openlocfilehash: 9b64a5e913748cda7927877c50a964ede587ae30
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026007"
---
# <a name="monitor-sql-data-sync-with-log-analytics"></a>Günlük analizi ile İzleyici SQL veri eşitleme 

SQL veri eşitleme Etkinlik günlüğünü denetleyin ve hataları ve Uyarıları algılamak için daha önce SQL veri eşitleme Azure portalında el ile denetleyin veya PowerShell veya REST API'yi kullanın zorunda kalındı. İzleme deneyimine veri eşitleme artıran özel bir çözümü yapılandırmak için bu makaledeki adımları izleyin. Bu çözüm senaryonuza uyacak şekilde özelleştirebilirsiniz.

SQL veri eşitleme genel bakış için bkz: [verileri Eşitle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme ile](sql-database-sync-data.md).

## <a name="monitoring-dashboard-for-all-your-sync-groups"></a>Tüm eşitleme grubu için izleme Panosu 

Artık tek tek sorunlar için aramak için her eşitleme grubu günlükleri göz gerekmez. Özel günlük analizi görünümü kullanarak herhangi bir yerde aboneliklerinizi tüm eşitleme grubu izleyebilirsiniz. Bu görünüm SQL veri eşitleme müşterilere önemli bilgileri de ortaya çıkarır.

![Veri Eşitleme izleme Panosu](media/sql-database-sync-monitor-oms/sync-monitoring-dashboard.png)

## <a name="automated-email-notifications"></a>Otomatik e-posta bildirimleri

Artık Azure portalında el ile veya PowerShell veya REST API aracılığıyla günlük denetlemeniz gerekir. İle [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), hata oluştuğunda bunları görmek için gereken kişilerin e-posta adreslerini doğrudan Git uyarılar oluşturabilirsiniz.

![Veri Eşitleme e-posta bildirimleri](media/sql-database-sync-monitor-oms/sync-email-notifications.png)

## <a name="how-do-you-set-up-these-monitoring-features"></a>Bu izleme özellikleri nasıl ayarladığınız? 

SQL veri eşitleme için kısa bir saat içinde şunları yaparak izleme çözümü özel bir günlük analizi uygulayın:

Üç bileşeni yapılandırmanız gerekir:

-   Günlük analizi SQL veri eşitleme günlük veri akışı için bir PowerShell runbook.

-   E-posta bildirimleri için günlük analizi uyarı.

-   Bir günlük analizi izleme görünümü.

### <a name="samples-to-download"></a>Karşıdan yüklemek için örnekleri

Aşağıdaki iki örnekleri indirin:

-   [Veri Eşitleme günlük PowerShell Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Veri Eşitleme günlük analizi görünümü](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki işlemleri ayarladığınızdan emin olun:

-   Bir Azure Otomasyonu hesabı

-   Log Analytics Çalışma Alanı

## <a name="powershell-runbook-to-get-sql-data-sync-log"></a>SQL veri eşitleme günlüğü almak için PowerShell Runbook 

SQL veri eşitleme günlük veri çekmek ve günlük Analizi'ne göndermek için Azure Otomasyonu'nda barındırılan bir PowerShell runbook kullanın. Bir örnek betik dahil edilir. Önkoşul olarak, bir Azure Otomasyonu hesabı olması gerekir. Ardından, bir runbook oluşturmak ve çalıştırmak üzere zamanlamak gerekir. 

### <a name="create-a-runbook"></a>Runbook oluşturma

Bir runbook oluşturma hakkında daha fazla bilgi için bkz: [ilk PowerShell runbook'um](https://docs.microsoft.com/azure/automation/automation-first-runbook-textual-powershell).

1.  Azure Otomasyonu hesabınızı altında seçin **Runbook'lar** işlem Otomasyonu sekmesinde.

2.  Seçin **Runbook Ekle** Runbook'lar sayfasının sol üst köşesinde adresindeki.

3.  Seçin **mevcut bir Runbook'u içeri aktar**.

4.  Altında **Runbook dosyası**, kullanın verilen `DataSyncLogPowerShellRunbook` dosya. Ayarlama **Runbook türü** olarak `PowerShell`. Runbook bir ad verin.

5.  **Oluştur**’u seçin. Şimdi bir runbook sahipsiniz.

6.  Azure Otomasyonu hesabınızı altında seçin **değişkenleri** sekmesi altında paylaşılan kaynakları.

7.  Seçin **değişken Ekle** değişkenleri sayfasında. Runbook için son yürütme zamanı depolamak için bir değişken oluşturun. Birden çok runbook varsa, her runbook için bir değişken gerekir.

8.  Değişken adı olarak ayarlanmış `DataSyncLogLastUpdatedTime` ve onun türü DateTime olarak ayarlayın.

9.  Runbook'u seçin ve sayfanın en üstündeki Düzenle düğmesini tıklatın.

10. Hesabınızı ve SQL veri eşitleme yapılandırması için gerekli değişiklikleri yapın. (Daha ayrıntılı bilgi için örnek komut dosyasına bakın.)

    1.  Azure Information.

    2.  Eşitleme grubu bilgileri.

    3.  OMS bilgileri. Bu bilgi OMS portalında | Ayarları | Bağlı kaynakları. Günlük analizi veri gönderme hakkında daha fazla bilgi için bkz: [HTTP veri toplayıcı API (Önizleme) ile günlük analizi veri Gönder](../log-analytics/log-analytics-data-collector-api.md).

11. Runbook Test Bölmesi'nde çalıştırın. Başarılı olup olmadığını denetleyin.

    Hatalar varsa, en son PowerShell modül yüklü olduğundan emin olun. En son PowerShell modülündeki yükleyebilirsiniz **modülleri galeri** Otomasyon hesabınızda.

12. Tıklatın **yayımlama**

### <a name="schedule-the-runbook"></a>Runbook zamanlama

Runbook'u zamanlamak için:

1.  Runbook'u altında seçin **zamanlamaları** Kaynaklar sekmesinde.

2.  Seçin **bir zamanlama Ekle** zamanlamaları sayfasında.

3.  Seçin **runbook'a bir zamanlama Bağla**.

4.  Seçin **yeni bir zamanlama oluşturun.**

5.  Ayarlama **yineleme** yinelenen ve küme için istediğiniz aralığı. Burada, aynı aralık OMS de komut dosyasını kullanın.

6.  **Oluştur**’u seçin.

### <a name="check-the-automation"></a>Otomasyon denetleyin

Automation'ınızı altında beklendiği gibi çalışıp çalışmadığını izlemek için **genel bakış** automation hesabınız için **Proje istatistikleri** altında görüntülemek **izleme**. Bu görünüm kolay görüntüleme için panonuza sabitleyin. Runbook Göster "Tamamlandı" olarak başarılı çalıştığında ve başarısız çalışır "Başarısız" Göster

## <a name="create-a-log-analytics-reader-alert-for-email-notifications"></a>E-posta bildirimleri için bir günlük analizi okuyucu uyarısı oluştur

Günlük analizi kullanan bir uyarı oluşturmak için şunları yapın. Bir önkoşul olarak günlük analizi günlük analizi çalışma alanı ile bağlantılı olması gerekir.

1.  OMS portalında seçin **günlük arama**.

2.  Seçtiğiniz aralık dahilinde hataları ve Uyarıları eşitleme grubu seçmek için bir sorgu oluşturun. Örneğin:

    `Type=DataSyncLog\_CL LogLevel\_s!=Success| measure count() by SyncGroupName\_s interval 60minute`

3.  Sorgu çalıştırdıktan sonra diyor zil seçin **uyarı**.

4.  Altında **uyarı Oluştur temel alarak**seçin **ölçüm ölçüm**.

    1.  Toplama değerini ayarlamak **büyük**.

    2.  Sonra **büyük**, bildirimler almadan önce geçecek eşiği girin. Geçici hataları veri eşitleme beklenir. Gürültü azaltmak için eşiği 5 olarak ayarlayın.

5.  Altında **Eylemler**ayarlayın **e-posta bildirimi** "Evet." İstenen e-posta alıcılarını girin.

6.  **Kaydet**’e tıklayın. Hata oluştuğunda belirtilen alıcılara e-posta bildirimleri artık alırsınız.

## <a name="create-an-oms-view-for-monitoring"></a>İzleme için bir OMS görünümü oluşturma

Bu adım, tüm belirtilen eşitleme grubu görsel olarak izlemek için bir OMS görünümü oluşturur. Görünüm çeşitli bileşenleri içerir:

-   Kaç tane hatalar, başarı ve uyarılar tüm eşitleme gruplarınız gösteren bir genel bakış kutucuğu.

-   Hataları ve Uyarıları eşitleme grubu başına sayısını gösteren bir bölme tüm eşitleme grubu için. Herhangi bir sorun gruplarıyla bu kutucuğa görünmüyor.

-   Her eşitleme hataları, başarı ve uyarılar ve en son hata iletileri sayısını gösterir grubu için bir kutucuğa.

OMS görünüm yapılandırmak için şunları yapın:

1.  OMS giriş sayfasında açmak için soldaki artı seçin **Görünüm Tasarımcısı**.

2.  Seçin **alma** Görünüm Tasarımcısı'nın üst çubukta. Ardından "DataSyncLogOMSView" örnek dosyayı seçin.

3.  Sample view iki eşitleme grubu yönetmek için kullanılır. Bu görünüm senaryonuza uyacak şekilde düzenleyin. Tıklatın **Düzenle** ve aşağıdaki değişiklikleri yapın:

    1.  Yeni "Halka & listesi" nesneleri Galeriden gerektiği şekilde oluşturun.

    2.  Her parçasında, sorguları bilgilerinizle güncelleştirin.

        1.  Her bölme üzerinde TimeStamp_t aralığını istediğiniz gibi değiştirebilirsiniz.

        2.  Her eşitleme grubu için bölmeler eşitleme grubu adları güncelleştirin.

    3.  Her bölme üzerinde başlığı gerektiği gibi güncelleştirin.

4.  Tıklatın **kaydetmek** ve görünüm hazırdır.

## <a name="cost-of-this-solution"></a>Bu çözüm maliyeti

Çoğu durumda, bu çözüm ücretsizdir.

**Azure Otomasyonu:** kullanımınıza bağlı olarak Azure Otomasyonu hesabı ile sonucunda oluşan bir maliyeti olabilir. İş yürütme süresi ayda ilk 500 dakikası ücretsizdir. Çoğu durumda, bu çözüm, 500 dakikadan daha kısa bir süre aylık kullanmak için bekleniyor. Ücretlerden kaçınmak için iki saat veya daha fazla bir aralıkla çalışması için runbook'u zamanlayın. Daha fazla bilgi için bkz: [Otomasyon fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/).

**Günlük analizi:** kullanımınıza bağlı olarak günlük analizi ile ilişkili bir maliyeti olabilir. Ücretsiz katmanı 500 MB günde alınan veri içerir. Çoğu durumda, bu çözüm günde 500 MB'tan az alma beklenir. Kullanımını azaltmak için yalnızca hata dahil filtreleme kullanın. 500 MB günde birden fazla kullanıyorsanız, sınırlaması ulaşıldığında durdurma analytics riskini önlemek için ücretli katmanına yükseltin. Daha fazla bilgi için bkz: [günlük analizi fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/).

## <a name="code-samples"></a>Kod örnekleri

Bu makalede aşağıdaki konumlardan açıklanan kod örnekleri indirin:

-   [Veri Eşitleme günlük PowerShell Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Veri Eşitleme günlük analizi görünümü](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

## <a name="next-steps"></a>Sonraki adımlar
SQL Data Sync hakkında daha fazla bilgi için bkz.:

-   [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md)
-   [Azure SQL Data Sync’i ayarlama](sql-database-get-started-sql-data-sync.md)
-   [Azure SQL Data Sync için en iyi yöntemler](sql-database-best-practices-data-sync.md)
-   [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)

-   SQL Data Sync’in nasıl yapılandırılacağını gösteren tam PowerShell örnekleri:
    -   [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [SQL Data Sync REST API belgelerini indirin](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

-   [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
-   [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
