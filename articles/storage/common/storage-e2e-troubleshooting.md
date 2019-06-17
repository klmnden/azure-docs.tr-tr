---
title: Azure depolama, tanılama ve ileti Çözümleyicisi ile sorun giderme | Microsoft Docs
description: Azure depolama analizi, AzCopy ve Microsoft ileti Çözümleyicisi ile uçtan uca sorun giderme gösteren bir öğretici
services: storage
author: normesta
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 03/15/2017
ms.author: normesta
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 2707081adafa74237e3fb7730837f581e0c8b790
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65154233"
---
# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Azure depolama ölçümlerini ve günlüğe kaydetme, AzCopy ve ileti Çözümleyicisi kullanarak uçtan uca sorun giderme
[!INCLUDE [storage-selector-portal-e2e-troubleshooting](../../../includes/storage-selector-portal-e2e-troubleshooting.md)]

Tanılama ve sorun giderme oluşturmak ve Microsoft Azure depolama ile istemci uygulamalarını desteklemek için temel bir yetenektir. Azure uygulaması dağıtılmış doğası nedeniyle, tanılama ve sorun giderme hataları ve performans sorunlarını geleneksel ortamlarda daha karmaşık olabilir.

Bu öğreticide, biz performansını etkiler ve bu hatalardan uçtan uca sorun giderme belirli hataları belirlemek nasıl göstermek istemci uygulaması en iyi duruma getirmek için Microsoft ve Azure Depolama tarafından sağlanan araçları kullanarak.

Bu öğretici, bir uçtan uca sorun giderme senaryo uygulamalı incelenmesi sağlar. Azure depolama uygulama sorunlarını giderme için bir ayrıntılı kavramsal kılavuz için bkz [izleme, tanılama ve sorun giderme Microsoft Azure depolama](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Azure depolama uygulamaları sorun giderme araçları
Microsoft Azure depolama kullanan istemci uygulamalar gidermek için ne zaman bir sorun oluştu ve bu sorunun nedeni ne olabilir belirlemek için birkaç aracı birden kullanabilirsiniz. Bu araçlar şunları içerir:

* **Azure depolama analizi**. [Azure depolama analizi](/rest/api/storageservices/Storage-Analytics) Ölçümler ve günlüğe kaydetme için Azure depolama sağlar.

  * **Depolama ölçümleri** işlem ölçümleri ve depolama hesabınız için kapasite ölçümlerini izler. Ölçümleri kullanarak, uygulamanızın farklı ölçü çeşitli göre nasıl performans gösterdiğini belirleyebilirsiniz. Bkz: [Storage Analytics Ölçüm tablosu şeması](/rest/api/storageservices/Storage-Analytics-Metrics-Table-Schema) depolama Analytics tarafından izlenen ölçümler türleri hakkında daha fazla bilgi için.
  * **Depolama günlüğü** her isteğin Azure Depolama Hizmetleri sunucu tarafı günlüğe kaydeder. Günlük gerçekleştirilen işlem dahil olmak üzere her bir istek, durumunu işlemi ve gecikme bilgileri için ayrıntılı verileri izler. Bkz: [depolama analizi günlük biçimi](/rest/api/storageservices/Storage-Analytics-Log-Format) depolama analizi tarafından günlüklere yazılır istek ve yanıt verilerini hakkında daha fazla bilgi.

* **Azure portalında**. Depolama hesabınız için Ölçümler ve günlüğe kaydetme yapılandırabilirsiniz [Azure portalında](https://portal.azure.com). Ayrıca, grafikler ve uygulamanızın zaman içinde nasıl performans gösterdiğini gösteren grafikler görüntüleyin ve uygulamanız için belirtilen bir ölçüm beklenenden farklı gerçekleştirdiğinde bunu size bildirecek uyarılar yapılandırın.

    Bkz: [Azure portalında depolama hesabı izleme](storage-monitor-storage-account.md) Azure portalında izlemeyi yapılandırma hakkında bilgi için.
* **AzCopy**. Azure depolama için sunucu günlüklerine, günlük bloblarını Microsoft ileti Çözümleyicisi'ni kullanarak analiz için yerel bir dizine kopyalamak için AzCopy kullanabilirsiniz blobları olarak depolanır. Bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md) AzCopy hakkında daha fazla bilgi.
* **Microsoft Message Analyzer**. İleti Çözümleyicisi günlük dosyalarını kullanır ve filtre, arama ve Grup günlük verileri için hataları ve performans sorunlarını analiz etmek için kullanabileceğiniz yararlı kümeleri halinde kolaylaştırır görsel bir biçimde günlük verileri görüntüleyen bir araçtır. Bkz: [Microsoft ileti Çözümleyicisi işletim kılavuzu](https://technet.microsoft.com/library/jj649776.aspx) ileti Çözümleyicisi hakkında daha fazla bilgi.

## <a name="about-the-sample-scenario"></a>Örnek senaryosu hakkında
Bu öğreticide, burada bir düşük yüzde başarı oranı Azure depolama çağıran bir uygulama için Azure depolama ölçümlerini gösterir bir senaryoyu inceleyeceğiz. Düşük yüzde başarı oranı ölçüm (olarak gösterilen **PercentSuccess** içinde [Azure portalında](https://portal.azure.com) ve tabulky Metrik), başarılı, ancak 299 büyük bir HTTP durum kodu iade işlemleri izler. Sunucu tarafı depolama günlük dosyalarında bir işlem durumuyla bu işlemleri kaydedilir **ClientOtherErrors**. Düşük başarı yüzdesi ölçüm hakkında daha fazla ayrıntı için bkz. [ölçümleri Göster düşük PercentSuccess veya Analiz günlük girişlerini sahip işlem durumundaki ClientOtherErrors işlemlerini](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure depolama işlemleri HTTP durum kodları 299 büyüktür normal işlevleri bir parçası olarak döndürebilir. Ancak bazı durumlarda bu hataları istemci uygulamanızın performansı için en iyi duruma getirmek mümkün olabilir.

Bu senaryoda, biz bir düşük yüzde başarı Oranı % 100 altındaki her şeyi olarak ele alacağız. Ancak, farklı bir ölçüm düzeyi ihtiyaçlarınıza göre seçebilirsiniz. Uygulamanızı test sırasında temel dayanıklılık için temel performans ölçümlerini oluşturmanızı öneririz. Örneğin, karar verebilir göre teste odaklandığından uygulamanızı %90 veya % 85'lik bir tutarlı yüzde başarı oranı olmalıdır. Ölçüler verilerinizi gösteren, uygulama, bu sayıdan deviating ardından artışa neden olabilecek araştırabilirsiniz.

Başarı yüzdesi oranı ölçüm %100 altında olduğundan emin ediyoruz kurduktan sonra örnek senaryomuz için ölçümleri ilişkilendirin ve bunları ne düşük yüzde başarı oranı neden olduğunu anlamak için kullanan hataları bulmak için günlüklere inceleyeceğiz. Özellikle 400 aralığındaki hatalar inceleyeceğiz. Ardından biz daha yakından 404 (bulunamadı) hataları araştırmanız.

### <a name="some-causes-of-400-range-errors"></a>400-range hatalarının bazı nedenleri
Aşağıdaki örneklerde, bazı 400-range hataların olası nedenlerin yanı sıra Azure Blob Depolama istekler için bir örnekleme gösterir. Bu hatalar, ek olarak 300 aralığı ve 500 aralığın hatalar herhangi bir düşük yüzde başarı oranı için katkıda bulunabilir.

Aşağıdaki listelerde tam gölgeden uzak olduğunu unutmayın. Bkz: [durumu ve hata kodları](https://msdn.microsoft.com/library/azure/dd179382.aspx) hataları her depolama hizmeti için özel ve genel Azure depolama hataları hakkında ayrıntılı bilgi için MSDN'de.

**Durum kodu 404 (bulunamadı) örnekleri**

Blob veya kapsayıcı bulunmadığı için bir kapsayıcı veya blob okuma işlemi başarısız olduğunda gerçekleşir.

* Bir kapsayıcı veya blob önce bu isteği başka bir istemci tarafından silinmiş olması durumunda gerçekleşir.
* Mevcut olup olmadığını denetledikten sonra kapsayıcı veya blob oluşturan bir API çağrısı kullanıyorsanız gerçekleşir. Createıfnotexists API'leri kapsayıcı veya blob varlığını denetlemek için önce bir baş çağrı yapın; yoksa, 404 hatası döndürülür ve ardından kapsayıcı veya blob yazmak için ikinci bir PUT çağrısı yapılır.

**Durum kodu: 409 (Çakışma) örnekleri**

* API Oluştur varlığını denetlemeden önce yeni bir kapsayıcı veya blob, oluşturmak için kullandığınız, bir kapsayıcı veya bu adı taşıyan blob zaten ortaya çıkar.
* Bir kapsayıcı silinir ve silme işlemi tamamlanmadan önce aynı ada sahip yeni bir kapsayıcı oluşturmak çalışırsanız oluşur.
* Bir kapsayıcı veya blob üzerinde kiralama belirtin ve zaten bir kira var ise oluşur.

**Durum kodu 412 (önkoşul başarısız oldu) örnekleri**

* Koşullu bir üst bilgisi tarafından belirtilen koşulu karşılanmadı oluşur.
* Kira kimliği belirtildi kapsayıcı veya blob kiralama Kimliğiyle eşleşmiyor oluşur.

## <a name="generate-log-files-for-analysis"></a>Çözümleme için günlük dosyalarını oluştur
Bunlardan herhangi biriyle çalışmaya seçebilirsiniz ancak bu öğreticide, Message Analyzer üç farklı türde günlük dosyaları ile çalışmak için kullanacağız:

* **Sunucu günlüğü**, Azure depolama günlüğü etkinleştirdiğinizde oluşturuldu. Sunucu günlüğü adlı bir Azure depolama hizmetleri - blob, kuyruk, tablo ve dosya karşı her bir işlem hakkında veriler içerir. Sunucu günlüğü işlemi çağrıldı ve hangi durum kodu istek ve yanıt ilgili döndürülen yanı sıra diğer ayrıntıları gösterir.
* **.NET istemci günlüğü**, .NET uygulama içinde istemci tarafı günlüğe etkinleştirdiğinizde oluşturuldu. İstemci günlük nasıl istemci isteği hazırlar ve alır ve yanıt işler hakkında ayrıntılı bilgi içerir.
* **HTTP ağ izleme günlüğü**, HTTP/HTTPS istek ve yanıt verilerini Azure Storage'a karşı işlemleri de dahil olmak üzere, veri toplar. Bu öğreticide, başlığında aracılığıyla ileti Çözümleyicisi kullanarak ağ izlemesini oluşturacağız.

### <a name="configure-server-side-logging-and-metrics"></a>Sunucu tarafı günlük kaydını ve ölçümleri yapılandırma
Böylece verileri analiz etmek için hizmet tarafı sahibiz ilk olarak, size Azure Storage günlüğe kaydetme ve ölçümler, yapılandırmanız gerekir. Aracılığıyla günlüğe kaydetme ve çeşitli şekillerde - ölçümlerde yapılandırabilirsiniz [Azure portalında](https://portal.azure.com), PowerShell kullanarak veya programlama yoluyla. Bkz [ölçümleri etkinleştirme](storage-analytics-metrics.md#enable-metrics-using-the-azure-portal) ve [günlüğe kaydetmeyi etkinleştirme](storage-analytics-logging.md#enable-storage-logging) günlüğe kaydetme ve ölçümler yapılandırma hakkında daha fazla ayrıntı için.

### <a name="configure-net-client-side-logging"></a>.NET istemci tarafı günlük kaydını yapılandırma
Bir .NET uygulaması için istemci tarafı günlük kaydını yapılandırmak için uygulamanın yapılandırma dosyasında (web.config veya app.config) .NET tanılamayı etkinleştirin. Bkz: [istemci-tarafı .NET depolama istemci kitaplığı ile günlüğe kaydetme](https://msdn.microsoft.com/library/azure/dn782839.aspx) ve [istemci-tarafı Java için Microsoft Azure depolama SDK'sı ile günlüğe kaydetme](https://msdn.microsoft.com/library/azure/dn782844.aspx) ayrıntılı bilgi için MSDN'de.

İstemci tarafı günlük nasıl istemci isteği hazırlar ve alır ve yanıt işler hakkında ayrıntılı bilgi içerir.

Depolama istemcisi kitaplığı, uygulamanın yapılandırma dosyasında (web.config veya app.config) belirtilen konumda istemci tarafı günlük verilerini depolar.

### <a name="collect-a-network-trace"></a>Ağ izleme Topla
İstemci uygulamanız çalışırken, bir HTTP/HTTPS ağ izleme toplamak için ileti Çözümleyicisi'ni kullanabilirsiniz. İleti Çözümleyicisi'ni kullanan [Fiddler](https://www.telerik.com/fiddler) arka uçta. Ağ izleme toplama önce şifrelenmemiş HTTPS trafiğini kaydetmek için fiddler'ı yapılandırmanızı öneririz:

1. Yükleme [Fiddler](https://www.telerik.com/download/fiddler).
2. Fiddler'ı başlatın.
3. Seçin **araçları | Fiddler seçenekleri**.
4. Seçenekler iletişim kutusunda, emin **yakalama HTTPS bağlanır** ve **HTTPS trafiği şifresini** her ikisi de, aşağıda gösterildiği gibi seçilir.

![Fiddler seçenekleri yapılandırın](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Öğretici için toplamak ve ağ izleme ileti Çözümleyicisi'nde ilk kaydedin, ardından izleme ve günlükleri analiz etmek için bir çözümleme oturumu oluşturun. İleti Çözümleyicisi'ndeki ağ izlemesi toplamak için:

1. İleti Çözümleyicisi'nde seçin **dosya | Hızlı İzleme | Şifrelenmemiş HTTPS**.
2. İzleme hemen uygulanmaya başlanır. Seçin **Durdur** böylece yalnızca izleme depolama trafiği için yapılandırabiliriz izlemeyi durdurmak için.
3. Seçin **Düzenle** izleme oturumu düzenlenecek.
4. Seçin **yapılandırma** sağındaki bağlantı **Microsoft Pef WebProxy** ETW sağlayıcısı.
5. İçinde **Gelişmiş ayarlar** iletişim kutusunda, tıklayın **sağlayıcısı** sekmesi.
6. İçinde **ana bilgisayar adı filtresi** alanında, boşluk ile ayırarak, depolama uç noktaları belirtin. Örneğin, uç noktalarınıza gibi belirtebilirsiniz; değiştirme `storagesample` depolama hesabınızın adı:

    ```   
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. İletişim kutusundan çıkmak ve tıklayın **yeniden** izlemede yalnızca Azure depolama ağ trafiği dahildir böylece izleme ile ana bilgisayar adı filtre yerden toplamaya.

> [!NOTE]
> Ağ izleme toplama tamamladıktan sonra HTTPS trafiği şifresini çözme Fiddler'da değiştirmiş olabilir ayarları geri kesinlikle öneririz. Fiddler Seçenekleri iletişim kutusunda seçimini **yakalama HTTPS bağlanır** ve **HTTPS trafiği şifresini** onay kutularını.
>
>

Bkz: [ağ izleme özelliklerini kullanmaya](https://technet.microsoft.com/library/jj674819.aspx) daha fazla bilgi için TechNet'teki.

## <a name="review-metrics-data-in-the-azure-portal"></a>Azure portalında ölçüm verileri gözden geçirin
Uygulamanızı bir süreliğine çalıştırıldıktan sonra görünen ölçüm grafikleri inceleyebilirsiniz [Azure portalında](https://portal.azure.com) hizmetinizin nasıl performans gösterdiğini gözlemleyin.

İlk olarak, Azure portalında depolama hesabınıza gidin. Bir izleme varsayılan olarak, grafik ile **başarı yüzdesi** ölçüm hesabı dikey penceresinde görüntülenir. Daha önce farklı ölçümleri görüntülemek için grafikteki değiştirdiyseniz, ekleme **başarı yüzdesi** ölçümü.

Artık göreceksiniz **başarı yüzdesi** izleme grafiğin yanı sıra başka bir ölçüm sizi eklemiş olabilecek. Biz sonraki ileti Çözümleyicisi'ndeki günlükleri analiz ederek araştırmak senaryosunda, başarı yüzdesi % 100 aşağıda biraz oranıdır.

Ekleme ve ölçüm grafikleri özelleştirme hakkında daha fazla bilgi için bkz [ölçüm grafikleri özelleştirme](storage-monitor-storage-account.md#customize-metrics-charts).

> [!NOTE]
> Bu ölçümler verilerinizin depolama ölçümleri etkinleştirdikten sonra Azure Portalı'nda görünmesi biraz zaman alabilir. Geçerli saat sona ermeden önceki bir saat saatlik ölçümlerini Azure portalında görüntülenmez olmasıdır. Ayrıca, dakika ölçümlerini şu anda Azure portalında görüntülenmez. Bu nedenle bağlı olarak ölçümleri etkinleştirdiğinizde, ölçüm verilerini görmek için iki saat sürebilir.
>
>

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Sunucu günlükleri, yerel bir dizine kopyalamak için AzCopy kullanma
Ölçümleri tablolara yazılır azure depolama BLOB'ları için sunucu günlüğü verileri yazar. İyi bilinen kullanılabilir günlük bloblarını `$logs` depolama hesabınız için kapsayıcı. Araştırmak istediğiniz zaman aralığını kolayca bulabilir, günlük bloblarını yıl, ay, gün ve saat tarafından hiyerarşik olarak adlandırılır. Örneğin, `storagesample` hesap, 01/02/2015 için gelen 8-9'da, günlük bloblarını için kapsayıcı `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. İle başlayarak, bu kapsayıcıdaki bloblara ardışık olarak adlandırılır `000000.log`.

Bu sunucu tarafı günlük dosyaları, yerel makinenizde seçtiğiniz bir konuma yüklemek için AzCopy komut satırı aracını kullanabilirsiniz. Örneğin, 2 Ocak 2015'te klasöre gerçekleşen blob işlemleri için günlük dosyalarını indirmek için aşağıdaki komutu kullanabilirsiniz `C:\Temp\Logs\Server`; değiştirin `<storageaccountname>` depolama hesabınızın adıyla ve `<storageaccountkey>` hesabınızın erişim anahtarıyla :

```azcopy
AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V
```
AzCopy, ücretsiz olarak kullanılabilir [Azure indirmeleri](https://azure.microsoft.com/downloads/) sayfası. AzCopy kullanma hakkında daha fazla ayrıntı için bkz. [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md).

Sunucu tarafı günlüklerini indirerek hakkında ek bilgi için bkz: [indirme günlüğü depolama günlük verilerini](https://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Günlük verilerini analiz etmek için Microsoft ileti Çözümleyicisi'ni kullanın
Microsoft Message Analyzer, yakalama, görüntüleme ve trafik, olayları ve diğer sorun giderme ve tanılama senaryoları sistem veya uygulama iletileri Mesajlaşma Protokolü çözümlemek için kullanılan bir araçtır. İleti Çözümleyicisi'ni yüklemek için toplama ve günlük verilerini analiz sağlar ve kaydedilen izleme dosyaları. İleti Çözümleyicisi hakkında daha fazla bilgi için bkz: [Microsoft ileti Çözümleyicisi işletim kılavuzu](https://technet.microsoft.com/library/jj649776.aspx).

İleti Çözümleyicisi varlıklar Azure depolama için sunucu, istemci ve ağ günlükleri analiz etmenize yardım içerir. Bu bölümde, depolama günlüklerinde düşük başarı yüzdesi, sorunu gidermek için bu araçları kullanmak nasıl açıklayacağız.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>İleti Çözümleyicisi'ni ve Azure depolama varlıkları indirip yükleyin
1. İndirme [Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) Microsoft İndirme Merkezi ve yükleyiciyi çalıştırın.
2. İleti Çözümleyicisi'ni başlatın.
3. Gelen **Araçları** menüsünde **varlık Yöneticisi**. İçinde **varlık Yöneticisi** iletişim kutusunda **indirir**, ardından filtre **Azure depolama**. Aşağıdaki resimde gösterildiği gibi Azure depolama varlıkları görürsünüz.
4. Tıklayın **eşitleme tüm görüntülenen öğelerin** Azure depolama varlıkları yüklemek için. Kullanılabilir Varlıklar içerir:
   * **Azure depolama renk kurallarını:** Azure depolama renk kurallarını içeren bir izleme özgü bilgiler iletileri vurgulamak için renk, metin ve yazı tipi stillerini kullanan özel filtreler tanımlamak etkinleştirin.
   * **Azure depolama grafikler:** Azure depolama grafikleri, sunucu günlük verilerini grafik önceden tanımlanmış grafikleri ' dir. Şu anda Azure depolama grafikleri kullanmak için yalnızca sunucu günlüğü analizi kılavuza yükleyebilir olduğunu unutmayın.
   * **Azure depolama Çözümleyicileri:** Azure depolama Çözümleyicileri, bunları analiz kılavuzunda görüntülemek için Azure depolama istemci, sunucu ve HTTP günlükleri ayrıştırılamıyor.
   * **Azure depolama filtreler:** Azure depolama filtreleri, verilerinizi analiz kılavuzunda sorgulamak için kullanabileceğiniz önceden tanımlanmış ölçütlerdir.
   * **Azure depolama görünüm düzenleri:** Azure depolama görünüm düzenleri şunlardır: önceden tanımlanmış sütun düzenleri ve analiz kılavuzunda gruplandırmaları.
5. Varlıkları yükledikten sonra ileti Çözümleyicisi'ni yeniden başlatın.

![İleti Çözümleyicisi varlık Yöneticisi](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [!NOTE]
> Tüm bu öğreticinin amaçları doğrultusunda gösterilen Azure depolama varlıkları yükleyin.
>
>

### <a name="import-your-log-files-into-message-analyzer"></a>Günlük dosyalarınızın ileti Çözümleyicisi alma
Tüm kayıtlı günlük dosyalarını (sunucu tarafı, istemci tarafı ve ağ), tek bir oturumda Microsoft ileti Çözümleyicisi çözümleme içine aktarabilirsiniz.

1. Üzerinde **dosya** Microsoft ileti Çözümleyicisi'nde menüsünü **yeni oturumu**ve ardından **boş oturumu**. İçinde **yeni oturumu** iletişim kutusunda, analiz oturumunuz için bir ad girin. İçinde **oturum ayrıntıları** panelinde, tıklayarak **dosyaları** düğmesi.
2. İleti Çözümleyicisi tarafından oluşturulan ağ izleme verileri yüklemek için tıklayın **Add Files**, web izleme oturumunuzdan .matp dosyanızın kaydedildiği konuma göz atın, .matp dosyasını seçin ve tıklayın **açın**.
3. Sunucu tarafı günlük verileri yüklemek için tıklayın **Add Files**, sunucu tarafı günlüklerinizi indirdiğiniz konuma gidin, analiz edin ve istediğiniz zaman aralığı için günlük dosyası seçmenizi **açık**. Ardından **oturum ayrıntıları** ayarlayın **metin günlüğü Yapılandırması** her sunucu tarafı günlük dosyasına için açılan **AzureStorageLog** emin olmak için Microsoft Message Çözümleyici günlük dosyası doğru şekilde ayrıştırabilirsiniz.
4. İstemci tarafı günlük verileri yüklemek için tıklayın **Add Files**, istemci tarafı günlüklerinizi kaydettiğiniz konuma gidin, analiz edin ve istediğiniz günlük dosyası seçmenizi **açık**. Ardından **oturum ayrıntıları** ayarlayın **metin günlüğü Yapılandırması** her istemci tarafı günlük dosyasına için açılan **AzureStorageClientDotNetV4** emin olmak için Microsoft Message Analyzer, günlük dosyası doğru şekilde ayrıştırabilirsiniz.
5. Tıklayın **Başlat** içinde **yeni oturumu** yüklenemedi ve günlük verilerini iletişim. İleti Çözümleyicisi çözümleme kılavuz günlük verilerini görüntüler.

Aşağıdaki resimde örnek bir oturum ile sunucu, istemci ve ağ izleme günlüğü dosyalarının yapılandırılmış gösterilmektedir.

![İleti Çözümleyicisi oturum yapılandırma](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

İleti Çözümleyicisi'ni günlük dosyaları belleğine yükler unutmayın. Günlük verileri büyük bir dizi varsa, ileti Çözümleyicisi'nden en iyi performansı almak için filtre uygulamak isteyeceksiniz.

İlk olarak isteyen bir zaman çerçevesi belirlemek ve bu zaman çerçevesindeki olabildiğince küçük tutun. Çoğu durumda, dakika veya saat en çok bir süre gözden geçirmek isteyebilirsiniz. En küçük kümesini ihtiyaçlarınızı karşılayabilecek günlükleri alın.

Günlük verileri büyük miktarda hala varsa, önce günlük verilerinizi yüklemek bir oturum filtresi belirlemek isteyebilirsiniz. İçinde **oturumu filtre** kutusunda **Kitaplığı** düğmesi önceden tanımlanmış bir filtre seçin; örneğin, **genel süresi filtre ı** Azure Depolama'dan filtreler filtrelemek için bir zaman aralığı. Başlangıç ve zaman damgası için görmek istediğiniz aralığı bitiş belirtmek üzere filtre kriterlerini daha sonra düzenleyebilirsiniz. Ayrıca, bir özel durum kodu filtreleyebilirsiniz; Örneğin, durum kodu 404 olduğu günlük girişlerini yüklemek seçebilirsiniz.

Microsoft Message Analyzer günlük verileri içeri aktarma hakkında daha fazla bilgi için bkz. [ileti verilerini alma](https://technet.microsoft.com/library/dn772437.aspx) TechNet'teki.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Günlük dosyası verilerini ilişkilendirmek için istemci İstek Kimliği'ni kullanın
Azure depolama istemci kitaplığı, benzersiz istemci istek kimliği her istek için otomatik olarak oluşturur. İleti Çözümleyicisi içindeki tüm üç günlükleri arasında verilerin bağıntısını kurmaya kullanabilmeniz için bu değer, istemci günlüğü, sunucu günlük ve ağ izleme için yazılır. Bkz: [istemci istek kimliği](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) istemci hakkında ek bilgi için kimliği isteyin.

Aşağıdaki bölümlerde önceden yapılandırılmış ve özel düzen görünümleri ilişkilendirmek için nasıl kullanılacağını açıklar ve grubu verileri istemci istek kimliğini temel alan

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Analiz ızgarada gösterilecek bir görünüm yerleşimini seçin
İleti Çözümleyicisi için depolama varlıkları verilerinizi yararlı gruplandırmaları ve farklı senaryolar için sütunları görüntülemek için kullanabileceğiniz önceden yapılandırılmış görünümler Azure depolama görünüm düzenleri içerir. Özel Görünüm düzenleri oluşturun ve bunları yeniden kullanmak üzere kaydedin.

Gösterir aşağıdaki resimde **görünüm yerleşimini** menüsünü seçerek kullanılabilir **görünüm yerleşimini** araç şeridinden. Azure depolama için Görünüm düzenleri altında gruplanır **Azure depolama** menüsünde düğümü. Arayabilirsiniz `Azure Storage` düzenleri yalnızca Azure depolama alanında filtrelemek için arama kutusunu görüntüleyin. Sık Kullanılanlara ve menüsünün üstünde görüntülemek için bir görünüm yerleşimini yanındaki yıldızı de seçebilirsiniz.

![Düzen menüsü görüntüleme](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Başlangıç seçin **Clientrequestıd'ye ve modül tarafından gruplandırılmış**. Bu görünümü Düzen üç günlüğü verileri ilk istemci istek kimliği, sonra bir kaynak günlük dosyasına günlük (veya **Modülü** ileti Çözümleyicisi). Bu görünümle, belirli bir istemci istek kimliği ile detaya gitme ve tüm üç günlük dosyalarını, istemci istek kimliği verileri görmek

Gösterir aşağıdaki resimde bu düzeni görünümünde görüntülenen sütunların bir alt kümesi ile örnek günlük verilere uygulanır. Belirli istemci istek kimliği için istemci günlüğü, sunucu günlük ve ağ izleme verilerini analiz kılavuz görüntüler görebilirsiniz.

![Azure depolama görünüm düzeni](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

> [!NOTE]
> Farklı günlük dosyaları sahip farklı sütunlar için birden çok günlük dosyası verilerini analiz kılavuz görüntülendiğinde, belirli bir satır için herhangi bir veri bazı sütunlar içeremez. Örneğin, yukarıdaki resimde, istemci günlük satırları için herhangi bir veri gösterme **zaman damgası**, **TimeElapsed**, **kaynak**, ve **hedef**sütunları, çünkü bu sütun istemci günlüğünde yok, ancak ağ izlemeye, mevcut. Benzer şekilde, **zaman damgası** sütunu zaman damgası sunucusu günlük verileri görüntüler, ancak hiçbir verinin gösterilmemesi için **TimeElapsed**, **kaynak**, ve  **Hedef** sunucu günlüğü parçası olmayan sütunlar.
>
>

Azure depolama görünüm düzenleri kullanmanın yanı sıra, ayrıca tanımlama ve kendi görünüm düzenleri kaydedin. Verileri gruplandırma için istenen diğer alanları seçebilir ve gruplandırmayı özel düzen de bir parçası olarak kaydedin.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Analiz kılavuza renk kurallarını uygula
Depolama varlıkları hataları analiz kılavuzdaki farklı türlerini tanımlamak üzere görsel anlamına gelir sunan renk kurallarını da içerir. Bunlar yalnızca sunucu günlüğü ve ağ izleme için görünmesi için HTTP hataları, önceden tanımlı renk kurallarını uygulayın.

Renk kurallarını uygulamak için seçin **renk kurallarını** araç şeridinden. Azure depolama renk kurallarını menüsünde görürsünüz. Öğretici için seçin **istemci hataları (durum kodu 400 ve 499 arasında)** , aşağıdaki resimde gösterildiği gibi.

![Azure depolama görünüm düzeni](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Azure depolama renk kurallarını kullanmanın yanı sıra, ayrıca tanımlama ve kendi renk kurallarını kaydedin.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>400-range hatalarını bulmak için Grup ve filtre günlük verileri
Ardından, Grup ve 400 aralığında tüm hataları bulmak için günlük verileri filtreleyin.

1. Bulun **StatusCode** analiz kılavuzunda sütun sağ tıklayın, sütun başlığını ve select **grubu**.
2. Ardından, gruplandırma **Clientrequestıd'ye** sütun. Veriler analiz kılavuzunda artık durum kodunu ve istemci istek kimliği tarafından düzenlenir görürsünüz.
3. Zaten görüntülenmiyorsa, Görünüm Filtresi Araç penceresini görüntüleyin. Araç şeridinde seçin **aracı Windows**, ardından **Görünümü Filtresi**.
4. Aşağıdaki filtre ölçütleri için günlük verileri yalnızca 400-range hataları görüntülemek için filtreye Ekle **Görünümü Filtresi** pencere seçeneğine tıklayıp **Uygula**:

    ```   
    (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)
    ```

Aşağıdaki resimde bu gruplandırma ve filtreleme sonuçları gösterilmektedir. Genişletme **Clientrequestıd'ye** durum kodu: 409, için olan gruplamayla altındaki alan, örneğin, bu durum kodunda sonuçlanan bir işlem gösterilir.

![Azure depolama görünüm düzeni](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Bu filtre uygulandıktan sonra göreceğiniz istemci günlüğü tablosundan satırları hariç tutulur, istemci olarak günlük içermemesi bir **StatusCode** sütun. Başlangıç olarak, biz sunucu ve 404 hatalarını bulmak için ağ izleme günlüklerini gözden geçireceğiz ve bunları yönetilen istemci işlemleri incelemek için istemci günlüğüne ardından getireceğiz.

> [!NOTE]
> Filtreleyebilirsiniz **StatusCode** sütun hala görünen verileri ve durum kodunu olduğu null günlük girişlerini içeren filtreye bir ifade eklerseniz istemci günlüğü dahil olmak üzere, tüm üç günlükleri. Bu filtre ifadesi oluşturmak için kullanın:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
>
> Bu filtre tüm satırları istemciden günlük ve yalnızca satır sunucu günlük ve HTTP günlüğü, durum kodu 400 ' büyük olduğu döndürür. İstemci istek kimliği ve modül tarafından gruplandırılmış görünümü Düzen uygulamanız durumunda, arama kaydırın veya üç tüm günlükler burada gösterilir olanları bulmak için günlüğü girdileri.   
>
>

### <a name="filter-log-data-to-find-404-errors"></a>404 hataları bulmak için günlük verileri filtreleme
Hataları veya aradığınız eğilimleri bulmak için günlük verileri daraltmak için kullanabileceğiniz önceden tanımlanmış filtreler depolama varlıkları içerir. Ardından, biz önceden tanımlanmış iki filtre uygulayacağınız: sunucu ve ağ izleme günlükleri 404 hataları filtreleyen ve belirtilen zaman aralığı verilerini filtreleyen bir.

1. Zaten görüntülenmiyorsa, Görünüm Filtresi Araç penceresini görüntüleyin. Araç şeridinde seçin **aracı Windows**, ardından **Görünümü Filtresi**.
2. Görüntüleme Filtresi penceresinde **Kitaplığı**ve arama `Azure Storage` Azure depolama bulmak için filtre uygular. İçin filtreyi seçin **404 (bulunamadı) tüm günlüklerde iletileri**.
3. Görüntü **Kitaplığı** menü yeniden bulun ve seçin **genel süresi filtre**.
4. Filtre, görüntülemek istediğiniz aralığı için gösterilen zaman damgaları düzenleyin. Bu verileri çözümlemek için çeşitli daraltmak için yardımcı olur.
5. Filtreniz, aşağıdaki örneğe benzer görünmelidir. Tıklayın **Uygula** analiz kılavuza filtre uygulamak için.

    ```   
    ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
    (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)
    ```

    ![Azure depolama görünüm düzeni](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Günlük verilerinizi analiz edin
Gruplandırılmış ve verilerinizi filtrelenmiş göre 404 hataları oluşturulan tek tek isteklerin ayrıntıları inceleyebilirsiniz. Geçerli Görünüm düzeni, veri, istemci istek kimliği, ardından günlük kaynak tarafından göre gruplandırılır. Biz isteklerinde 404 burada içeren StatusCode alan filtre olduğundan, yalnızca sunucu ve ağ izleme verilerini, istemci günlük verilerini göreceğiz.

Aşağıdaki resimde, burada blob mevcut olmadığından bir Blob alma işlemi bir 404 hatası veriyor, belirli bir istek gösterir. Bazı sütunların standart görünümünden ilgili verileri görüntülemek için kaldırıldığına dikkat edin.

![Filtrelenen sunucu ve ağ izleme günlükleri](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Ardından, biz bu istemci istek kimliği hata oluştuğunda erişilmiş istemci sürüyordu hangi eylemleri görmek için istemci günlük verilerin bağıntısını. İkinci bir sekmede açılır istemci günlük verilerini görüntülemek bu oturum için yeni bir analiz kılavuz görünümü görüntüleyebilirsiniz:

1. İlk olarak, değerini kopyalayın **Clientrequestıd'ye** panoya alan. Bunu yapmak için ya da satır seçilmesi, bulma **Clientrequestıd'ye** alan, veri değeri sağ tıklayıp seçme **Kopyala 'Clientrequestıd'ye'** .
2. Araç şeridinde seçin **yeni Görüntüleyici**, ardından **analiz kılavuz** yeni bir sekmede açmak için. Yeni sekme tüm verileri gruplandırma, filtreleme veya renk kurallarını içermeyen günlük dosyalarınızı gösterir.
3. Araç şeridinde seçin **görünüm yerleşimini**, ardından **tüm .NET istemci sütunları** altında **Azure depolama** bölümü. Bu görünüm yerleşimini, sunucu ve ağ izleme günlüklerini yanı sıra log istemci verileri gösterir. Varsayılan olarak üzerinde sıralanır **Lowmessages** sütun.
4. Ardından, istemci istek kimliği için istemci günlük arama Araç şeridinde seçin **iletileri Bul'u**, ardından özel bir filtre istemci istek Kimliğini belirtin **Bul** alan. Kendi istemci istek kimliği belirleme filtresi için aşağıdaki sözdizimini kullanın:

    ```
    *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"
    ```

İleti Çözümleyicisi'ni bulur ve burada arama ölçütleriyle eşleşen istemci isteği kimliği ilk günlük girişi seçer İstemci oturum açma vardır her istemci istek kimliği için birden çok girişi, üzerinde gruplamak istediğiniz şekilde **Clientrequestıd'ye** tümünü bir araya görebileceği daha kolay hale getirmek için alan. Aşağıdaki resimde tüm iletileri istemci oturum açma için belirtilen istemci istek kimliği gösterir

![İstemci günlük gösteren 404 hataları](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Görünüm düzenleri bu iki sekme olarak gösterilen verileri kullanarak hataya neyin neden olduğunu belirlemek için istek verileri analiz edebilirsiniz. Bunu görmek için 404 hatası yönlendirebilecek önceki bir olay için öncesinde istekleri da göz atabilirsiniz. Örneğin, bu istemci İstek Kimliği ' blob silinmiş olabilir veya hata nedeniyle istemci uygulama bir kapsayıcı veya blob bir Createıfnotexists API'ye çağrı yapma ise belirlemek için önceki istemci günlük girişlerini gözden geçirebilirsiniz. İstemci günlüğünde blobun adresini bulabilirsiniz **açıklama** alan; bu bilgileri sunucu ve ağ izleme günlükleri görünür **özeti** alan.

404 hatası veriyor blob adresini öğrendikten sonra daha ayrıntılı bir araştırma. Günlük girişlerini aynı blob işlemleri ile ilişkili diğer iletiler için arama yaparsanız, istemci varlık daha önce silinmiş olup olmadığını kontrol edebilirsiniz.

## <a name="analyze-other-types-of-storage-errors"></a>Diğer türde depolama hatalarını çözümleme
Günlük verilerinizi analiz etmek için ileti Çözümleyicisi kullanımıyla ilgili bilgi sahibi olduğunuza göre diğer türde bir görünümü kullanarak hataları çözümleyebilir düzenleri, renk kurallarını ve arama ve filtreleme. Aşağıdaki tablolarda, karşılaşabileceğiniz bazı sorunları ve bunların yerini bulmak için kullanabileceğiniz filtreleme ölçütlerini listeler. Filtreler ve dil filtreleme Message Analyzer'ı oluşturma ile ilgili daha fazla bilgi için bkz: [ileti veri filtreleme](https://technet.microsoft.com/library/jj819365.aspx).

| Araştırmak için... | Filtre ifadesi kullan... | İfade günlüğüne uygular (istemci, sunucu, ağ, tüm) |
| --- | --- | --- |
| Kuyrukta ileti tesliminde beklenmeyen gecikmeler |"Yeniden deneniyor, işlem başarısız oldu." AzureStorageClientDotNetV4.Description içerir |İstemci |
| HTTP Percentthrottlingerror'da artış |HTTP.Response.StatusCode   == 500 &#124;&#124; HTTP.Response.StatusCode == 503 |Ağ |
| Percenttimeouterror'da artış artırın |HTTP.Response.StatusCode   == 500 |Ağ |
| (Tümü) Percenttimeouterror'da artış artırın |\* StatusCode 500 == |Tümü |
| Percentnetworkerror'da artış artırın |AzureStorageClientDotNetV4.EventLogEntry.Level   < 2 |İstemci |
| HTTP 403 (Yasak) iletileri |HTTP.Response.StatusCode   == 403 |Ağ |
| HTTP 404 (bulunamadı) iletileri |HTTP. Response.StatusCode 404 == |Ağ |
| 404 (Tümü) |\* Durum kodu 404 == |Tümü |
| Paylaşılan erişim imzası (SAS) yetkilendirme sorunu |AzureStorageLog.RequestStatus "SASAuthorizationError" == |Ağ |
| 409 (Çakışma) HTTP iletileri |HTTP.Response.StatusCode   == 409 |Ağ |
| 409 (Tümü) |\* StatusCode 409 == |Tümü |
| Düşük PercentSuccess veya Analiz günlük girişlerini ClientOtherErrors işlem durumundaki işlemlerini sahip |AzureStorageLog.RequestStatus ==   "ClientOtherError" |Sunucusu |
| Nagle Uyarısı |((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) ve (AzureStorageLog.RequestPacketSize < 1460) ve (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200) |Sunucusu |
| Sunucu ve ağ günlüklerinde zaman aralığı |#Timestamp > = 2014-10-20T16:36:38 ve #Timestamp < = 2014-10-20T16:36:39 |Sunucu, ağ |
| Sunucu günlükleri, zaman aralığı |AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 ve AzureStorageLog.Timestamp < = 2014-10-20T16:36:39 |Sunucusu |

## <a name="next-steps"></a>Sonraki adımlar
Azure Depolama'daki sorun giderme uçtan uca senaryolar hakkında daha fazla bilgi için şu kaynaklara bakın:

* [Microsoft Azure Depolama izleme, tanılama ve sorun giderme](storage-monitoring-diagnosing-troubleshooting.md)
* [Depolama Analizi](https://msdn.microsoft.com/library/azure/hh343270.aspx)
* [Azure portalında depolama hesabı izleme](storage-monitor-storage-account.md)
* [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](storage-use-azcopy.md)
* [Microsoft Message Analyzer işletim kılavuzu](https://technet.microsoft.com/library/jj649776.aspx)
