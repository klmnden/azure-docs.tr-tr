---
title: SSIS tümleştirme çalışma zamanı paketi yürütme sorunlarını giderme | Microsoft Docs
description: Bu makalede SSIS tümleştirme çalışma zamanı SSIS paketi yürütme için sorun giderme kılavuzu sağlar.
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
ms.openlocfilehash: 68a5d5278e1181695695647cff187d4b95624b40
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537640"
---
# <a name="troubleshoot-package-execution-in-the-ssis-integration-runtime"></a>SSIS tümleştirme çalışma zamanı paketi yürütme sorunlarını giderme

Bu makale, SSIS tümleştirme çalışma zamanı'nda SQL Server Integration Services (SSIS) paketlerini yürütülürken zaman kullanabileceğiniz en yaygın hataları içerir. Bu hataları gidermek için olası nedenler ve eylemleri açıklar.

## <a name="where-to-find-logs-for-troubleshooting"></a>Sorun giderme amacıyla günlükleri nerede bulacağını

SSIS paketi yürütme etkinliğinin çıkış denetlemek için Azure Data Factory portal'ı kullanın. Çıktı yürütme sonucu, hata iletileri ve işlem kimliğini içerir. Ayrıntılar için bkz [işlem hattını izleme](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline).

Yürütme için ayrıntılı günlükleri denetlemek için SSIS kataloğunu (SSISDB) kullanın. Ayrıntılar için bkz [İzleyici çalışan paketler ve diğer işlemleri](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017).

## <a name="common-errors-causes-and-solutions"></a>Yaygın hatalar, nedenleri ve çözümleri

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Hata iletisi: "Bağlantı zaman aşımı süresi doldu" veya "hizmet isteğinizi işlerken bir hatayla karşılaştı. Lütfen yeniden deneyin."

Olası nedenler ve önerilen eylemler şunlardır:
* Veri kaynağı veya hedefi aşırı yüklendi. Yük veri kaynağı veya hedefi denetleyin ve yeterli kapasiteye sahip olup olmadığını görebilirsiniz. Örneğin, Azure SQL veritabanı kullandıysanız, veritabanı için zaman aşımı olasılığı varsa ölçeği artırmayı düşünün.
* Özellikle bağlantı bölgeler arası veya şirket içi ile Azure arasında olduğunda SSIS tümleştirme çalışma zamanı ve veri kaynağı veya hedefi arasında kararsız, ağdır. SSIS paketi içinde yeniden deneme düzeni, aşağıdaki adımları izleyerek geçerlidir:
  * SSIS paketlerinizi hata durumunda (örneğin, veri kaybı veya veri çoğaltma) yan etkileri olmadan çalıştırabilirsiniz emin olun.
  * Yapılandırma **yeniden** ve **yeniden deneme aralığı** , **SSIS paketi yürütme** faaliyete **genel** sekmesi. ![Genel sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
  * Bir ADO.NET ve OLE DB kaynak veya hedef bileşeni için ayarlanan **ConnectRetryCount** ve **ConnectRetryInterval** SSIS paketi veya SSIS etkinliğini Bağlantı Yöneticisi'nde.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Hata iletisi: "ADO NET kaynak bağlantı almak '...' başarısız oldu" "bir ağla ilgili veya örneğe özel bir SQL Server bağlantısı kurulurken hata oluştu ile. Sunucu bulunamadı veya erişilebilir durumda değildi."

Bu sorun genellikle veri kaynağı anlamına gelir veya hedef SSIS tümleştirme çalışma zamanını şuradan erişilebilir değil. Nedeniyle değişebilir. Bu eylemler deneyin:
* Veri kaynağı veya hedefi geçirme emin adı/IP doğru.
* Güvenlik duvarının düzgün şekilde ayarlandığından emin olun.
* Şirket içi veri kaynağı veya hedefi ise sanal ağınızda düzgün yapılandırıldığından emin olun:
  * Aynı sanal ağdaki bir Azure VM sağlayarak sorunu sanal ağ yapılandırmasından olup olmadığını doğrulayabilirsiniz. Veri kaynağı veya hedefi Azure VM'den erişilebilir olup olmadığını denetleyin.
  * SSIS tümleştirme çalışma zamanı içinde bir sanal ağ kullanma hakkında daha fazla ayrıntı bulabilirsiniz [bir Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katılın](join-azure-ssis-integration-runtime-virtual-network.md).

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-could-not-create-a-managed-connection-manager"></a>Hata iletisi: "ADO NET kaynak bağlantı almak '...' başarısız oldu" "yönetilen bir bağlantı Yöneticisi oluşturulamadı."

Pakette kullanılan ADO.NET sağlayıcısının SSIS tümleştirme çalışma zamanı'nda yüklü olmadığını raporlayabilir olası nedeni. Sağlayıcı özel kurulum kullanarak yükleyebilirsiniz. Özel kurulumda hakkında daha fazla ayrıntı bulabilirsiniz [Azure-SSIS tümleştirme çalışma zamanı Kurulum özelleştirme](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="error-message-the-connection--is-not-found"></a>Hata iletisi: "Bağlantı '...' bulunamadı"

Bilinen bir sorun SQL Server Management Studio (SSMS) daha eski sürümlerinde bu hataya neden olabilir. Paket dağıtımı yapmak SSMS'yi kullanıldığı makinede yüklü olmayan bir özel bileşene (örneğin, SSIS Azure Feature Pack veya iş ortağı bileşenleri) içeriyorsa, SSMS bileşeni kaldırmak ve hataya neden. Yükseltme [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) sabit sorunu en son sürümü için.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Hata iletisi: "Yeterli alanı yok diskte"

Bu hata, yerel disk SSIS Integration runtime düğümü kullanıldığı anlamına gelir. Disk alanı çok fazla paket veya özel kurulum tüketiyor olup olmadığını kontrol edin:
* Paketiniz tarafından kullanılan disk, paket yürütme sona erdikten sonra yedekleme boşaltılacak.
* Özel kurulumunuzu tarafından tüketilen disk varsa, SSIS tümleştirme çalışma zamanını durdurmak için kodunuzu değiştirmek ve Integration runtime'ı yeniden başlatın. SSIS Integration runtime düğümü için özel kurulum kopyalanacak için belirttiğiniz tüm Azure blob kapsayıcısı, bu nedenle denetleyin gereksiz içerikler bu kapsayıcı altında olup olmadığını.

### <a name="error-message-cannot-open-file-"></a>Hata iletisi: "Dosyası açılamıyor '...'"

Paket yürütme SSIS tümleştirme çalışma zamanı yerel diskteki bir dosya bulamadığında bu hata oluşur. Bu eylemler deneyin:
* Mutlak yol SSIS tümleştirme çalışma zamanı'nda yürütülen paketteki kullanmayın. Geçerli yürütme çalışma dizinine (.) veya temp klasörü (% TEMP %) kullanın Bunun yerine.
* SSIS tümleştirme çalışma zamanı düğümleri üzerinde bazı dosyaların kalıcı olması gerekiyorsa, açıklandığı dosyaları hazırlama [Kurulum özelleştirme](how-to-configure-azure-ssis-ir-custom-setup.md). Yürütme sona erdikten sonra çalışma dizinindeki tüm dosyaların temizlenir.
* Azure dosyaları, dosya SSIS tümleştirme çalışma zamanı düğümü depolamak yerine kullanın. Ayrıntılar için bkz [kullanımı Azure dosya paylaşımlarını](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares).

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Hata iletisi: "'SSISDB' veritabanı boyut kotasına ulaştı"

Olası bir nedeni, Azure SQL veritabanı veya yönetilen bir örneği bir SSIS tümleştirme çalışma zamanı oluştururken oluşturulan SSISDB veritabanı kotasına ulaştı olmasıdır. Bu eylemler deneyin:
* Veritabanı DTU artırmayı deneyin. Ayrıntılı bilgi bulabilirsiniz [Azure SQL veritabanı sunucusu için SQL veritabanı kaynak limitleri](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server).
* Paketiniz birçok günlükler üretir olup olmadığını denetleyin. Bu durumda, bu günlüklerini temizlemek için esnek bir iş yapılandırabilirsiniz. Ayrıntılar için bkz [Azure elastik veritabanı işleri ile SSISDB günlükleri Temizle](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md).

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Hata iletisi: "Veritabanı için istek sınırı olan... ve üst sınırına ulaşıldı."

Birçok paketi SSIS tümleştirmesi çalışma zamanında paralel çalıştırıyorsanız, SSISDB istek sınırına ulaşıp ulaşmadığını çünkü bu hata oluşabilir. DTC, bu sorunu çözmek için SSISDB artırmayı deneyin. Ayrıntılı bilgi bulabilirsiniz [Azure SQL veritabanı sunucusu için SQL veritabanı kaynak limitleri](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server).

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Hata iletisi: "SSIS işlem beklenmeyen işlem durumu ile başarısız oldu:..."

Hata genellikle geçici bir sorundan kaynaklanır, böylece paket yürütmeyi yeniden deneyin. SSIS paketi içinde yeniden deneme düzeni, aşağıdaki adımları izleyerek geçerlidir:

* SSIS paketlerinizi hata durumunda (örneğin, veri kaybı veya veri çoğaltma) yan etkileri olmadan çalıştırabilirsiniz emin olun.
* Yapılandırma **yeniden** ve **yeniden deneme aralığı** , **SSIS paketi yürütme** faaliyete **genel** sekmesi. ![Genel sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
* Bir ADO.NET ve OLE DB kaynak veya hedef bileşeni için ayarlanan **ConnectRetryCount** ve **ConnectRetryInterval** SSIS paketi veya SSIS etkinliğini Bağlantı Yöneticisi'nde.

### <a name="error-message-there-is-no-active-worker"></a>Hata iletisi: "Etkin bir çalışan sağlıyor."

Bu hata, genellikle SSIS tümleştirme çalışma zamanının düzgün çalışmayan bir durumu sahip olduğu anlamına gelir. Ayrıntılı hatalar ve durumu için Azure portalını denetleyin. Daha fazla bilgi için [Azure-SSIS tümleştirme çalışma zamanı](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Hata iletisi: "Integration runtime yükseltilemez ve sonunda biz için özel kurulum sağlanan Azure Blob kapsayıcısına erişilemiyor. bu yana, çalışmayı durdurur."

SSIS tümleştirme çalışma zamanı için özel kurulum yapılandırılmış depolama erişemediğinde, bu hata oluşur. Paylaşılan erişim imzası (SAS), sağlanan URI geçerli olduğundan ve süresinin dolmadığından olup olmadığını denetleyin.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Hata iletisi: "Analysis Services için Microsoft OLE DB sağlayıcısı. ' Hresult: 0x80004005 açıklaması:' COM hatası: COM hatası: mscorlib; Bir çağırma hedefi tarafından özel durum oluşturuldu"

Olası nedenlerinden biri, kullanıcı adı veya parola ile etkin bir Azure multi-Factor Authentication, Azure Analysis Services kimlik doğrulaması için yapılandırılmış olmasıdır. Bu kimlik doğrulaması, SSIS tümleştirme çalışma zamanı'nda desteklenmiyor. Azure Analysis Services kimlik doğrulaması için bir hizmet sorumlusu kullanmayı deneyin:
1. Bölümünde anlatıldığı gibi bir hizmet sorumlusu hazırlama [hizmet sorumluları ile Otomasyon](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal).
2. Bağlantı Yöneticisi'nde yapılandırma **belirli bir kullanıcı adı ve parolayı kullanın**: ayarlayın **AppID** kullanıcı adı olarak ve **clientSecret** parolası.

### <a name="error-message-adonet-source-has-failed-to-acquire-the-connection-guid-with-the-following-error-message-login-failed-for-user-nt-authorityanonymous-logon-when-using-a-managed-identity"></a>Hata iletisi: "{GUID} bağlantısı aşağıdaki hata iletisini almaya ADONET kaynağı başarısız oldu: Kullanıcısı 'NT Yetkili\Anonim Oturum açma' için oturum açılamadı "bir yönetilen kimlik kullanırken

Kimlik doğrulama yöntemi olarak Bağlantı Yöneticisi'nin yapılandırma yok emin **Active Directory parola kimlik doğrulaması** olduğunda parametresi *ConnectUsingManagedIdentity* olduğu **True** . Olarak yapılandırabileceğiniz **SQL kimlik doğrulaması** bunun yerine, hangi sayılır *ConnectUsingManagedIdentity* ayarlanır.

### <a name="package-execution-takes-too-long"></a>Paket yürütme çok uzun sürüyor

Olası nedenler ve önerilen eylemler şunlardır:
* Çok fazla paket yürütme SSIS tümleştirme çalışma zamanını zamanladınız. Bu yürütme için kendi bırakma kuyrukta bekleniyor.
  * En fazla şu formülle belirler: 
    
    IR başına en fazla Paralel yürütme sayısı düğüm sayısı = * düğüm başına en fazla Paralel yürütme
  * Düğüm sayısı ve düğüm başına en fazla Paralel yürütme hakkında bilgi edinmek için bkz: [Azure Data Factory'de bir Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md).
* SSIS tümleştirme çalışma zamanı durdurulmuş veya sağlıksız bir durumda. SSIS tümleştirme çalışma zamanı durum ve hataları denetlemek nasıl öğrenmek için bkz. [Azure-SSIS tümleştirme çalışma zamanı](monitor-integration-runtime.md#azure-ssis-integration-runtime).

Ayrıca üzerinde bir zaman aşımı ayarlamanızı öneririz **genel** sekmesinde: ![Genel sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png).

### <a name="poor-performance-in-package-execution"></a>Paket yürütme performansın düşmesine

Bu eylemler deneyin:

* SSIS Integration runtime veri kaynağı ve hedef aynı bölgede olduğundan emin olun.

* Paket yürütme için günlük tutma düzeyini ayarlamaya **performans** her bir bileşende yürütme süresi bilgi toplamak için. Ayrıntılar için bkz [Integration Services (SSIS) günlüğe kaydetme](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging).

* Azure portalında IR düğümü performansını kontrol edin:
  * SSIS tümleştirme çalışma zamanını izleme hakkında daha fazla bilgi için bkz: [Azure-SSIS tümleştirme çalışma zamanı](monitor-integration-runtime.md#azure-ssis-integration-runtime).
  * Azure portalında data Factory ölçümleri görüntüleyerek SSIS tümleştirme çalışma zamanı için CPU/bellek içi geçmiş bulabilirsiniz.
    ![SSIS tümleştirme çalışma zamanı ölçümlerini izleme](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
