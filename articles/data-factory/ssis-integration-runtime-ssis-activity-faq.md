---
title: SSIS tümleştirme çalışma zamanı paketi yürütme sorunlarını giderme | Microsoft Docs
description: Bu makalede SSIS paketi yürütme SSIS tümleştirme çalışma zamanı için sorun giderme kılavuzu sağlar.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/15/2019
author: wenjiefu
ms.author: wenjiefu
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: f17c364d258ef356a98180c9903603d92a6a9245
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078527"
---
# <a name="troubleshooting-package-execution-in-ssis-integration-runtime"></a>SSIS tümleştirme çalışma zamanı paketi yürütmeye ilişkin sorun giderme

Bu makale, SSIS tümleştirme çalışma zamanı, olası nedenleri ve Eylemler hataları çözmek için yürütme SSIS paketleri yükleyen isabet en yaygın hataları içerir.

## <a name="where-can-i-find-logs-for-troubleshoot"></a>Sorun giderme için günlükleri nerede bulabilirim

* SSIS paketi yürütme yürütme sonucu, hata iletileri ve işlem kimliği de dahil olmak üzere etkinlik çıkışı denetlemek için ADF portalı kullanılabilir. Ayrıntıları bulunabilir [işlem hattını izleme](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline)

* SSIS kataloğunu (SSISDB), yürütme için ayrıntılı günlükleri denetlemek için kullanılabilir. Ayrıntı bulunabilir [İzleyici çalışan paketler ve diğer işlemler](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017)

## <a name="common-errors-causes-and-solution"></a>Yaygın hatalar, neden ve çözüm

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Hata iletisi: `"Connection Timeout Expired."` veya `"The service has encountered an error processing your request. Please try again."`

* Olası neden & önerilen eylem:
  * Kaynak/hedef veri aşırı yüklendi. Üzerinde veri kaynak/hedef yükünü denetleyin ve yeterli kapasiteye sahip olup olmadığını görebilirsiniz. Örneğin, Azure SQL kullandıysanız, veritabanını zaman aşımına olasılığı varsa ölçek yukarı göz önünde bulundurmanız önerilir.
  * Özellikle bağlantı bölgeler arası veya şirket içi ile azure arasında olduğunda SSIS tümleştirme çalışma zamanı ve veri kaynak/hedef arasında kararsız, ağdır. SSIS paketi içinde yeniden deneme düzeni uygulamak için aşağıdaki adımları önerilir:
    * SSIS paketlerinizi yan etkisi olmadan hata durumunda (örneğin çalıştırabilirsiniz emin olun veri kaybı, veri dup...)
    * Yapılandırma **yeniden** ve **yeniden deneme aralığı** Genel sekmesinde SSIS paketi Etkinlik yürütme ![Genel sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
    * ADO.NET ve OLEDB kaynak/hedef bileşen ConnectRetryCount ConnectRetryInterval Bağlantı Yöneticisi'nde SSIS paketi veya SSIS etkinliğini ayarlanabilir

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-the-following-error-message-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Hata iletisi: `"ADO NET Source has failed to acquire the connection '...' with the following error message: "A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible."`

* Olası neden & önerilen eylem:
  * Bu sorun genellikle anlamına gelir kaynak/hedef veri öğesinden farklı nedenlerden kaynaklanabilir SSIS Integration Runtime erişilemez durumda:
    * Veri kaynak/hedef adı/IP doğru geçirme emin olun
    * Güvenlik duvarının düzgün şekilde ayarlandığından emin olun
    * Sanal ağınızı şirket içinde veri kaynak/hedef olması durumunda düzgün yapılandırıldığından emin olun.
      * Bir Azure VM aynı vNet içindeki sağlayarak sorunu vNet yapılandırmasından olup olmadığını doğrulayabilirsiniz. Ardından veri kaynak/hedef Azure sanal makinesinden erişilebilir olup olmadığını denetleyin.
      * VNet SSIS tümleştirmesi çalışma zamanında kullanma hakkında daha fazla ayrıntı bulabilirsiniz [bir Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katılın](join-azure-ssis-integration-runtime-virtual-network.md)

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-the-following-error-message-could-not-create-a-managed-connection-manager"></a>Hata iletisi: "`ADO NET Source has failed to acquire the connection '...' with the following error message: "Could not create a managed connection manager.`"

* Olası neden & önerilen eylem:
  * Pakette kullanılan ADO.NET sağlayıcısının SSIS tümleştirme çalışma zamanı yüklü değil. Sağlayıcı özel kurulum kullanarak yükleyebilirsiniz. Özel ayarları hakkında daha fazla ayrıntı bulunabilir [Azure-SSIS tümleştirme çalışma zamanı Kurulum özelleştirme](how-to-configure-azure-ssis-ir-custom-setup.md)

### <a name="error-message-the-connection--is-not-found"></a>Hata iletisi: "`The connection '...' is not found`"

* Olası neden & önerilen eylem:
  * Bu hatanın nedeni olabilir SSMS eski sürümündeki bilinen bir sorundur. Paket dağıtımı yapmak SSMS'yi kullanıldığı makinede yüklü olmayan bir özel bileşene (örneğin, SSIS Azure Feature Pack veya 3. taraf bileşenlerden) içeriyorsa, bileşen tarafından SSMS kaldırılacak ve hataya neden. Yükseltme [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) sabit sorunu en son sürümü için.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Hata iletisi: "Yeterli alanı yok diskte"

* Olası neden & önerilen eylem:
  * Bu hata, yerel disk SSIS Integration Runtime düğümünde kullanıldığı anlamına gelir. Paket veya özel kurulum çok sayıda disk alanları kullanılmasına neden olur olup olmadığını denetleyin.
    * Paketiniz tarafından kullanılan disk, paket yürütme sona erdikten sonra yedekleme boşaltılacak.
    * Özel kurulumunuzu tarafından tüketilen disk, SSIS tümleştirme çalışma zamanını durdurmak, kodunuzu değiştirmek ve SSIS Integration Runtime'ı yeniden başlatın gerekecektir. Tüm Azure Blob kapsayıcısı için özel kurulum belirtilen üzerinden SSIS IR düğüme kopyalanır, bu nedenle bu kapsayıcı altında gereksiz herhangi bir içerik olup olmadığını doğrulayın.

### <a name="error-message-cannot-open-file-"></a>Hata iletisi: "Dosyası açılamıyor '...'"

* Olası neden & önerilen eylem:
  * Paket yürütme SSIS tümleştirme çalışma zamanı yerel disk dosyası bulamadığında bu hata oluşur.
    * Mutlak yol SSIS tümleştirme çalışma zamanı yürütme paketi kullanmak için önerilen değil. Geçerli yürütme çalışma dizinine (.) veya geçici klasörü (% TEMP %) kullanın Bunun yerine.
    * SSIS tümleştirme çalışma zamanı düğümleri bazı dosyaları kalıcı hale getirmek gerekli olan, dosyaları hazırlamak için önerilir [özelleştirme Kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). Yürütme sona erdikten sonra yürütme çalışma dizinindeki tüm dosyaların temizlenir.
    * Başka bir seçenek, SSIS Integration Runtime düğümünde dosya depolamak yerine Azure dosya kullanmaktır. Daha fazla ayrıntı şu adreste bulunabilir: [ https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares ](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares).

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Hata iletisi: "'SSISDB' veritabanı boyut kotasına ulaştı"

* Olası neden & önerilen eylem:
  * Azure SQL veya yönetilen örneği, SSIS tümleştirme çalışma zamanı oluşturma sırasında oluşturulan SSISDB kotasına ulaştı.
    * Bu sorunu çözmek için veritabanının DTU artırmayı deneyin. Ayrıntılar şu yolda bulunabilir: [https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server)
    * Paketiniz birçok günlükler üretir olup olmadığını denetleyin. Bu durumda, elastik iş bu günlüklerini temizlemek için yapılandırılabilir. Başvurmak [Azure elastik veritabanı işleri ile SSISDB günlükleri Temizle](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md) ayrıntısı için.

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Hata iletisi: "Veritabanı için istek sınırı olan... ve üst sınırına ulaşıldı."

* Olası neden & önerilen eylem:
  * Paralel SSIS tümleştirme çalışma zamanı içinde birçok paketi çalıştırıldığında SSISDB isteği SORUMLULUĞUN isabet çünkü bu hata oluşabilir. Bu sorunu çözmek için SSISDB DTC artırmayı deneyin. Ayrıntılar şu yolda bulunabilir: [https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server)

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Hata iletisi: "SSIS işlem beklenmeyen işlem durumu ile başarısız oldu:..."

* Olası neden & önerilen eylem:
  * Geçici bir hata ile çoğunlukla hataya, böylece paket yürütmeyi yeniden deneyin. SSIS paketi içinde yeniden deneme düzeni uygulamak için aşağıdaki adımları önerilir:
    * SSIS paketlerinizi hata durumunda (örneğin, veri kaybı, veri dup....) yan etkisi olmadan çalıştırabilirsiniz emin olun
    * Yapılandırma **yeniden** ve **yeniden deneme aralığı** Genel sekmesinde SSIS paketi Etkinlik yürütme ![Genel sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
    * ADO.NET ve OLEDB kaynak/hedef bileşen ConnectRetryCount ConnectRetryInterval Bağlantı Yöneticisi'nde SSIS paketi veya SSIS etkinliğini ayarlanabilir

### <a name="error-message-there-is-no-active-worker"></a>Hata iletisi: "Etkin bir çalışan sağlıyor."

* Olası neden & önerilen eylem:
  * Bu hata genellikle SSIS tümleştirme çalışma zamanı sağlıksız durumda anlamına gelir. Durum ve ayrıntılı hatalar için Azure portalını denetleyin: [https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Hata iletisi: "Integration runtime yükseltilemez ve sonunda biz için özel kurulum sağlanan Azure Blob kapsayıcısına erişilemiyor. bu yana, çalışmayı durdurur."

* SSIS tümleştirme çalışma zamanı için özel kurulum yapılandırılmış depolama erişemediğinde, bu hata oluşur. Sağlanan SAS URI'sinin geçerli olduğundan ve süresinin dolmadığından denetleyin.

### <a name="package-takes-unexpected-long-time-to-execute"></a>Paket beklenmeyen uzun zaman alabilir.

* Olası neden & önerilen eylem:
  * Çok fazla paket yürütme SSIS tümleştirme çalışma zamanını zamanladınız. Bu durumda, bu yürütme ve dolayısıyla yürütmek sırada bekleyen.
    * IR başına en fazla Paralel yürütme sayısı düğüm sayısı = * düğüm başına en fazla Paralel yürütme
    * Başvurmak [Azure Data factory'de Azure-SSIS Integration Runtime Oluştur](create-azure-ssis-integration-runtime.md) için düğüm başına en fazla Paralel yürütme ve düğüm sayısını ayarlama.
  * SSIS tümleştirme çalışma zamanı durduruldu veya sağlıksız bir durumda. Denetleme [Azure-SSIS tümleştirme çalışma zamanı](monitor-integration-runtime.md#azure-ssis-integration-runtime) nasıl SSIS tümleştirme çalışma zamanı durum ve hataları denetleyin.
  * Paket yürütmeyi belirli bir süre içinde tamamlanması eminseniz zaman aşımını ayarlamanız önerilir: ![Genel sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

### <a name="poor-performance-in-package-execution"></a>Paket yürütme performansın düşmesine

* Olası neden & önerilen eylem:

  * SSIS Integration Runtime veri kaynağı ve hedef aynı bölgede olup olmadığını denetleyin.

  * "Performans" Günlük düzeyi etkinleştir

      Her bir bileşende yürütme için daha fazla ayrıntı süresi bilgi toplamak için "performans" Paket yürütmeyi günlüğe kaydetme düzeyini ayarlayabilirsiniz. Ayrıntılar, yolda bulunabilir: [https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging)

  * Azure portalında izleme sayfası IR IR düğümü performansını kontrol edin.
    * SSIS tümleştirme çalışma zamanını izleme nasıl: [Azure-SSIS tümleştirme çalışma zamanı](monitor-integration-runtime.md#azure-ssis-integration-runtime)
    * Geçmiş SSIS Integration Runtime'nın CPU/bellek kullanımı Azure portalında Data Factory ölçümlere kullanılabilir ![SSIS tümleştirme çalışma zamanı ölçümlerini izleme](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
