---
title: Azure Log Analytics içinde depolanan kişisel verilere yönelik kılavuz | Microsoft Docs
description: Bu makalede, tanımlamak ve kaldırmak için Azure Log Analytics ve yöntemleri depolanan kişisel verileri yönetme açıklar.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: magoedte
ms.openlocfilehash: 0cf5a80e3eedbe7efb8463162b5b3ed489ac08c8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61087296"
---
# <a name="guidance-for-personal-data-stored-in-log-analytics-and-application-insights"></a>Log Analytics ve Application Insights depolanan kişisel verilere yönelik kılavuz

Log Analytics, kişisel veriler bulunma olasılığı olduğu bir veri deposudur. Application Insights, Log Analytics bölümünde verilerini depolar. Bu makalede, Log Analytics ve Application Insights bu tür veriler genellikle, bu tür verileri işlemek için özellikleri yanı sıra bulunduğu ele alınacaktır.

> [!NOTE]
> Bu makalenin amaçları için _günlük verilerini_ bir Log Analytics çalışma alanına gönderilen verilerin başvuruyor ancak _uygulama verileri_ Application Insights tarafından toplanan verileri ifade eder.

[!INCLUDE [gdpr-dsr-and-stp-note](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Kişisel veri işleme stratejisi

Bu, şirketinizin en fazla ve sonuçta ile özel işleyecek stratejisini belirlemek için veri (varsa) ancak bazı olası yaklaşımlar aşağıda verilmiştir. Bir teknik açısından en fazla tercih sırasına göre listelenen en az tercih için:

* Mümkünse, koleksiyonunu durdurma, karartmak, Anonimleştir veya aksi "özel" kabul dışlanacak toplanmakta olan verileri ayarlayın. Bu _popüleri_ işleme çok pahalı ve etkili bir veri oluşturmaya gerek kaydetme, tercih edilen yaklaşım.
* Mümkün olduğunda veri platformu ve performans üzerindeki etkiyi azaltmak için veri'leri normalleştirmek çalışır. Örneğin, günlük kaydı açık bir kullanıcı kimliği yerine kullanıcı adı ve ardından başka bir yerde kaydedilebilecek bir iç kimliği ayrıntılarının bağıntısını bir arama verileri oluşturun. Böylece, kullanıcılarınız kendi kişisel bilgilerinin silmenize istemelisiniz yalnızca kullanıcıya karşılık gelen arama tablosundaki satırın silinmesi yeterli olacağını mümkündür. 
* Son olarak, özel veri toplanması gereken, işlem temizleme API yol ve dışa aktarma ve bir kullanıcıyla ilişkili herhangi bir özel veri siliniyor olabilir yükümlülükleri karşılamak için var olan sorgu API'si yolu oluşturun. 

## <a name="where-to-look-for-private-data-in-log-analytics"></a>Log analytics'te özel verileri aramak nerede?

Log Analytics'in, verilerinizin bir şemaya prescribing çalışırken her alanı özel değerlerle geçersiz olanak tanıyan esnek deposu bulunur. Ayrıca, herhangi bir özel şema aktarılabilir. Bu nedenle, tam olarak nerede özel veriler belirli çalışma alanınızda bulunacaktır söyleyin mümkün değildir. Aşağıdaki konumlarda ancak iyi envanterinize başlangıç noktaları:

### <a name="log-data"></a>Günlük verileri

* *IP adresleri*: Log Analytics, birçok farklı tablolar arasında çeşitli IP bilgileri toplar. Örneğin, aşağıdaki sorgu tüm tabloları IPv4 adresleri son 24 saat boyunca toplanan burada gösterilmektedir:
    ```
    search * 
    | where * matches regex @'\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(\.|$)){4}\b' //RegEx originally provided on https://stackoverflow.com/questions/5284147/validating-ipv4-addresses-with-regexp
    | summarize count() by $table
    ```
* *Kullanıcı kimliklerini*: Çok çeşitli çözümler ve tablolar kullanıcı kimlikleri bulundu. Belirli bir kullanıcı adı için arama komutunu kullanarak, veri kümesi genelinde arayabilirsiniz:
    ```
    search "[username goes here]"
    ```
  Yalnızca kullanıcı tarafından okunabilen kullanıcı adları aynı zamanda doğrudan geri belirli bir kullanıcıya izlenebilir GUID'leri için aranacak unutmayın!
* *Cihaz kimlikleri*: "Kullanıcı kimlikleri gibi cihaz kimlikleri bazen özel" olarak kabul edilir. Tabloları tanımlamak için kullanıcı kimlikleri için yukarıda listelenen bölgelere aynı yaklaşımı kullanmak olduğunda bu bir sorun olabilir. 
* *Özel veri*: Log Analytics'e sağlayan çeşitli yöntemler koleksiyonda: özel günlükleri ve özel alanları [HTTP veri toplayıcı API'sini](../../azure-monitor/platform/data-collector-api.md) , ve özel veri, sistem olay günlüklerini bir parçası olarak toplanır. Bunların tümü, özel veri içeren açıktır ve herhangi bir veri var olup olmadığını doğrulamak için incelenmelidir.
* *Çözüm Yakalanan veriler*: Çözüm mekanizması açık uçlu bir tane olduğundan, uyumluluk sağlamak için çözümler tarafından oluşturulan tüm tabloları incelemeniz önerilir.

### <a name="application-data"></a>Uygulama verileri

* *IP adresleri*: Application Insights varsayılan olarak "0.0.0.0" tüm IP adresi alanlarıyla karartmak, ancak oturum bilgilerini korumak için gerçek kullanıcı IP bu değerle geçersiz kılmak için oldukça sık kullanılan bir desendir. Aşağıdaki Analytics sorgusu, son 24 saat boyunca "0.0.0.0" dışındaki IP adresi sütundaki değerleri içeren herhangi bir tabloda bulmak için kullanılabilir:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *Kullanıcı kimliklerini*: Varsayılan olarak, kullanıcı ve oturum izleme için Application Insights rastgele oluşturulmuş kimlikleri kullanır. Ancak, bir kimliği uygulamayla ilgili daha fazla saklamak için geçersiz kılınan bu alanları görmek için yaygındır. Örneğin: kullanıcı adları, AAD GUID'leri, vs. Bu kimlik genellikle olarak değerlendirilir kapsamındaki kişisel verileri olarak ve bu nedenle, uygun şekilde yapılması gerekir. Karartmak veya bu kimliklerinin Anonimleştir denemek için her zaman bizim kullanılması önerilir. Burada bu değerleri yaygın olarak bulunan alanları session_ıd, USER_ID, user_AuthenticatedId, user_AccountId yanı sıra customDimensions içerir.
* *Özel veri*: Application Insights için herhangi bir veri türü bir dizi özel boyutlar eklenecek sağlar. Bu boyutlara olabilir *herhangi* veri. Son 24 saat boyunca toplanan herhangi bir özel boyutlar tanımlamak için aşağıdaki sorguyu kullanın:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Bellek ve aktarım sırasında verileri*: Application Insights, özel durumlar, istekler, bağımlılık çağrıları ve izlemeler izler. Özel veriler genellikle kod ve HTTP çağrısı düzeyinde toplanabilir. Özel durumlar, istekler, bağımlılıklar ve izlemeleri tabloları tür verileri tanımlamak için gözden geçirin. Kullanım [telemetri başlatıcılar](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) mümkün olduğunda bu verileri karartmak.
* *Anlık görüntü hata ayıklayıcısı yakalamaları*: [Snapshot Debugger](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) özellik Application ınsights'da uygulamanızı üretim örneği üzerinde bir özel durum yakalandı her hata ayıklama anlık görüntüleri toplamak sağlar. Anlık görüntüleri, özel durumların yanı sıra, yığındaki her adımda yerel değişkenler için değerler baştaki tam yığın izlemesi açığa çıkarır. Ne yazık ki, bu özellik ek noktalarından veya anlık görüntü verileri programlı erişim seçmeli silme işlemi için izin vermez. Bu nedenle, varsayılan anlık görüntü elde tutma oranı, uyumluluk gereksinimlerini karşılamadığı, özelliği devre dışı bırakmak için kullanılması önerilir.

## <a name="how-to-export-and-delete-private-data"></a>Dışarı aktarma ve silme özel veriler

Belirtildiği gibi [kişisel veri işleme stratejisi](#strategy-for-personal-data-handling) bölümüne, __kesin__ if koleksiyonunu devre dışı bırakmak için veri toplama İlkesi yapılandırılacak olası tüm, önerilir Aksi takdirde "özel" kabul kaldırmak için değiştirme obfuscating veya onu anonymizing özel veriler. Size ve ekibinize tanımlayın ve otomatikleştirmek için bir strateji, bir arabirim verileriyle etkileşim kurmak müşterileriniz için derleme maliyetlerini ve devam eden bakım maliyetlerini veri en önemli sonucu işleme. Ayrıca, Log Analytics ve Application Insights ve yüksek hacimde eş zamanlı sorgu hesaplama açısından pahalı veya temizleme API çağrılarının tüm Log Analytics işlevselliği etkileşim olumsuz olasılığına sahiptir. Başka bir deyişle, burada özel veri gerekir toplanmasını geçerli bazı senaryolar vardır. Bu durumlarda, bu bölümde açıklanan şekilde veri yapılmalıdır.

[!INCLUDE [gdpr-intro-sentence](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Görüntüle ve dışarı aktarma

Hem görüntüleme hem de veri isteklerini dışarı aktarmak için [Log Analytics sorgu API'si](https://dev.loganalytics.io/) veya [Application Insights sorgu API'si](https://dev.applicationinsights.io/quickstart) kullanılmalıdır. Kullanıcılarınıza sunmak için uygun bir veri şekli dönüştürmek için mantıksal uygulamanız size olacaktır. [Azure işlevleri](https://azure.microsoft.com/services/functions/) böyle bir mantık barındırmak için harika bir yer sağlar.

> [!IMPORTANT]
>  Temizleme işlemleri büyük çoğunluğu SLA'sı çok hızlı tamamlarken **resmi 30 günde temizleme işlemlerinin tamamlanmasını SLA ayarlanır** yoğun kullanılan bir veri platformu etkilerini nedeniyle. Bu otomatik bir işlemdir; bir işlem daha hızlı işlenmesi istemek için hiçbir yolu yoktur.

### <a name="delete"></a>Sil

> [!WARNING]
> Log analytics'te siler yıkıcı ve çevrilemez! Lütfen son derece dikkatli olun, bunların yürütme kullanın.

Yaptık kullanılabilir gizlilik işlemlerinin bir parçası olarak bir *Temizleme* API yolu. Bu yol, böylece ile ilişkili riski nedeniyle olabildiğince az kullanılmalıdır olası performans etkisini ve olası tüm toplamalar, Ölçümler ve Log Analytics verilerinizi diğer yönleri eğriltmek için. Bkz: [kişisel veri işleme stratejisi](#strategy-for-personal-data-handling) özel verileri işlemek alternatif yaklaşımlar için bölüm.

Temizleme, uygulama ya da kullanıcı (bile kaynak sahibi dahil) azure'daki açıkça alınmadan verilen Azure Kaynak Yöneticisi'ndeki Rol yürütmek için izinlere sahip üst düzeyde ayrıcalıklı bir işlemdir. Bu rol _veri Purger_ ve olası veri kaybını nedeniyle dikkatli Devredilmiş olması. 

Azure Resource Manager rol atandıktan sonra iki yeni API yolları kullanılabilir: 

#### <a name="log-data"></a>Günlük verileri

* [Temizleme sonrası](https://docs.microsoft.com/rest/api/loganalytics/workspaces%202015-03-20/purge) - silinecek verilerin parametrelerini belirten nesnesini alır ve bir başvuru GUID'sini döndürür 
* GET temizleme durumu - POST temizleme çağrısına temizleme API'nizi durumunu belirlemek için çağıran bir URL içeren bir 'x-ms-durumunu-location' üst bilgisi döndürür. Örneğin:

    ```
    x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/Microsoft.OperatonalInsights/workspaces/[WorkspaceName]/operations/purge-[PurgeOperationId]?api-version=2015-03-20
    ```

> [!IMPORTANT]
>  SLA'mız, Log Analytics tarafından kullanılan veri platformu ağır etkilerini nedeniyle daha hızlı tamamlanması büyük çoğunluğu temizleme işlemleri bekliyoruz sırada **resmi 30 günde temizleme işlemlerinin tamamlanmasını SLA ayarlanır**. 

#### <a name="application-data"></a>Uygulama verileri

* [Temizleme sonrası](https://docs.microsoft.com/rest/api/application-insights/components/purge) - silinecek verilerin parametrelerini belirten nesnesini alır ve bir başvuru GUID'sini döndürür
* GET temizleme durumu - POST temizleme çağrısına temizleme API'nizi durumunu belirlemek için çağıran bir URL içeren bir 'x-ms-durumunu-location' üst bilgisi döndürür. Örneğin:

   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

> [!IMPORTANT]
>  Temizleme işlemleri büyük çoğunluğu ağır Application Insights tarafından kullanılan veri platformu etkilerini nedeniyle SLA çok hızlı tamamlarken **resmi 30 günde temizleme işlemlerinin tamamlanmasını SLA ayarlanır**.

## <a name="next-steps"></a>Sonraki adımlar
- Log Analytics verilerini nasıl toplanan, işlenen ve güvenliği hakkında daha fazla bilgi için bkz: [Log Analytics veri güvenliği](../../azure-monitor/platform/data-security.md).
- Application Insights verilerini nasıl toplanan, işlenen ve güvenliği hakkında daha fazla bilgi için bkz: [Application Insights veri güvenliği](../../azure-monitor/app/data-retention-privacy.md).
