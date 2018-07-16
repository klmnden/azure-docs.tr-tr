---
title: Azure Application Insights içinde depolanan kişisel verilere yönelik kılavuz | Microsoft Docs
description: Bu makalede, tanımlamak ve kaldırmak için Azure Application Insights ve yöntemleri depolanan kişisel verileri yönetme açıklar.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.reviewer: Evgeny.Ternovsky
ms.author: mbullwin
ms.openlocfilehash: a59b57c546f18a7d91160f2ae7282af82fc42160
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044722"
---
# <a name="guidance-for-personal-data-stored-in-application-insights"></a>Uygulama anlayışları'nda depolanan kişisel verilere yönelik kılavuz

Application Insights, kişisel veriler bulunma olasılığı olduğu bir veri deposu. Bu makalede, Application Insights'ta bu veriler genellikle, bu verileri işlemek için özellikleri yanı sıra bulunduğu açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Kişisel veri işleme stratejisi

Bu, şirketinizin en fazla ve sonuçta ile özel işleyecek stratejisini belirlemek için veri (varsa) ancak bazı olası yaklaşımlar aşağıda verilmiştir. Bir teknik açısından en fazla tercih sırasına göre listelenen en az tercih için:
* Mümkünse, koleksiyonunu durdurma, karartmak, Anonimleştir ve aksi takdirde dikkate alınmasını dışlamak için toplanan verileri ihtiyaçlarımızı _özel_. Bu yöntem işleme pahalı ve etkili bir veri oluşturmaya gerek kaydetme tercih edilen bir yaklaşım olan.
* Mümkün olduğunda veri platformu ve performans üzerindeki etkiyi azaltmak için veri'leri normalleştirmek çalışır. Örneğin, günlük kaydı açık bir kullanıcı kimliği yerine kullanıcı bağıntısını verilere arama ve bunların ayrıntılarını sonra başka bir yerde kaydedilebilecek bir iç kimliği oluşturun. Kullanıcılarınız, kendi kişisel bilgilerini silmek için sorarsanız bu şekilde, yalnızca kullanıcıya karşılık gelen arama tablosundaki satırın silinmesi yeterli olacağını mümkündür. 
* Son olarak, özel veri toplanması gereken, işlem temizleme API yol ve dışa aktarma ve bir kullanıcıyla ilişkili herhangi bir özel veri siliniyor olabilir yükümlülükleri karşılamak için var olan sorgu API'si yolu oluşturun.

## <a name="where-to-look-for-private-data-in-application-insights"></a>Application ınsights'ta özel veri aramak nerede?

Application Insights, her alanı özel değerlerle geçersiz olanak tanıyan, verilerinizin bir şemaya prescribing çalışırken tamamen esnek bir depo. Bu nedenle, tam olarak nerede özel veriler belirli uygulamanızda bulunacaktır söyleyin mümkün değildir. Aşağıdaki konumlarda ancak iyi envanterinize başlangıç noktaları:

* *IP adresleri*: sırasında Application Insights varsayılan olarak karartmak "0.0.0.0" için tüm IP adresi alanları, oturum bilgilerini korumak için gerçek kullanıcı IP bu değerle geçersiz kılmak için oldukça sık kullanılan bir desendir. Aşağıdaki Analytics sorgusu, son 24 saat boyunca "0.0.0.0" dışındaki IP adresi sütundaki değerleri içeren herhangi bir tabloda bulmak için kullanılabilir:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *Kullanıcı kimliklerini*: varsayılan olarak, Application Insights rastgele oluşturulmuş kimlikleri kullanıcı ve oturum izleme için kullanır. Ancak, bir kimliği uygulamayla ilgili daha fazla saklamak için geçersiz kılınan bu alanları görmek için yaygındır. Örneğin: kullanıcı adları, AAD GUID'leri, vs. Bu kimlik genellikle olarak değerlendirilir kapsamındaki kişisel verileri olarak ve bu nedenle, uygun şekilde yapılması gerekir. Karartmak veya bu kimliklerinin Anonimleştir denemek için her zaman bizim kullanılması önerilir. Burada bu değerleri yaygın olarak bulunan alanları session_ıd, USER_ID, user_AuthenticatedId, user_AccountId yanı sıra customDimensions içerir.
* *Özel veri*: Application Insights, herhangi bir veri türü bir dizi özel boyutlar eklenecek olanak sağlar. Bu boyutlara olabilir *herhangi* veri. Son 24 saat boyunca toplanan herhangi bir özel boyutlar tanımlamak için aşağıdaki sorguyu kullanın:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Bellek ve aktarım sırasında verileri*: Application Insights, özel durumlar, istekler, bağımlılık çağrıları ve izlemeler izleyecek. Özel veriler genellikle kod ve HTTP çağrısı düzeyinde toplanabilir. Özel durumlar, istekler, bağımlılıklar ve izlemeleri tabloları tür verileri tanımlamak için gözden geçirin. Kullanım [telemetri başlatıcılar](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) mümkün olduğunda bu verileri karartmak.
* *Anlık görüntü hata ayıklayıcısı yakalamaları*: [Snapshot Debugger](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) özellik Application ınsights'da uygulamanızı üretim örneği üzerinde bir özel durum yakalandı her hata ayıklama anlık görüntüleri toplamak sağlar. Anlık görüntüleri, özel durumların yanı sıra, yığındaki her adımda yerel değişkenler için değerler baştaki tam yığın izlemesi açığa çıkarır. Ne yazık ki, bu özellik ek noktalarından veya anlık görüntü verileri programlı erişim seçmeli silme işlemi için izin vermez. Bu nedenle, varsayılan anlık görüntü elde tutma oranı, uyumluluk gereksinimlerini karşılamadığı, özelliği devre dışı bırakmak için kullanılması önerilir.

## <a name="how-to-export-and-delete-private-data"></a>Dışarı aktarma ve silme özel veriler

Bu __kesin__ önerilir, olası obfuscating veya anonymizing, özel veri koleksiyonunu devre dışı bırakmak için veri toplama İlkesi yapılandırılacak veya aksi "özel" kabul kaldırmak için değiştirme Sonra toplanan., size ve ekibinize tanımlayın ve otomatikleştirmek için bir strateji, bir arabirim verileriyle etkileşim kurmak müşterileriniz için derleme maliyetlerini ve devam eden bakım maliyetlerine neden olur, veri işleme. Ayrıca, Application Insights ve yüksek hacimde eş zamanlı sorgu hesaplama açısından pahalı veya temizleme API çağrıları Application Insights işlevselliğe sahip diğer tüm etkileşim olumsuz olasılığına sahiptir. Başka bir deyişle, burada özel veri gerekir toplanmasını geçerli bazı senaryolar vardır. Bu durumlarda, bu bölümde açıklanan şekilde veri yapılmalıdır.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Görüntüle ve dışarı aktarma

Hem görüntüleme hem de veri isteklerini dışarı aktarmak için [sorgu API'si](https://dev.applicationinsights.io/quickstart) kullanılmalıdır. Kullanıcılarınıza sunmak için uygun bir veri şekli dönüştürmek için mantıksal uygulamanız size olacaktır. [Azure işlevleri](https://azure.microsoft.com/services/functions/) böyle bir mantık barındırmak için harika bir yerdir.

### <a name="delete"></a>Sil

> [!WARNING]
> Application ınsights'ta siler yıkıcı ve çevrilemez! Lütfen son derece dikkatli olun, bunların yürütme kullanın.

Bir "temizleme" API yolu hikaye gizlilik işlemlerinin bir parçası olarak kullanılabilir gerçekleştirdik. Bu yol, böylece ile ilişkili riski nedeniyle olabildiğince az kullanılmalıdır olası performans etkisini ve olası tüm toplamalar, Ölçümler ve Application Insights verilerini diğer yönleri eğriltmek için. Bkz: [kişisel veri işleme stratejisi](#strategy-for-personal-data-handling) özel verileri işlemek alternatif yaklaşımlar için bölüm yukarıda.

Temizleme, uygulama ya da kullanıcı (bile kaynak sahibi dahil) azure'daki açıkça alınmadan verilen Azure Kaynak Yöneticisi'ndeki Rol yürütmek için izinlere sahip üst düzeyde ayrıcalıklı bir işlemdir. Bu rol _veri Purger_ ve olası veri kaybını nedeniyle dikkatli bir şekilde Devredilmiş olması.

Azure Resource Manager rol atandıktan sonra iki yeni API yolları kullanılabilir ve tam Geliştirici belgeleri bağlıdır ve bağlı şekli çağırın:

* [Temizleme sonrası](https://docs.microsoft.com/rest/api/application-insights/components/purge) - silinecek verilerin parametrelerini belirten nesnesini alır ve bir başvuru GUID'sini döndürür
* GET temizleme durumu - POST temizleme çağrısına temizleme API'nizi durumunu belirlemek için çağıran bir URL içeren bir 'x-ms-durumunu-location' üst bilgisi döndürür. Örneğin:
   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

> [!IMPORTANT]
>  Temizleme işlemleri büyük çoğunluğu ağır Application Insights tarafından kullanılan veri platformu etkilerini nedeniyle SLA çok hızlı tamamlarken **resmi 30 günde temizleme işlemlerinin tamamlanmasını SLA ayarlanır**.

## <a name="next-steps"></a>Sonraki adımlar
Verileri nasıl toplanan, işlenen ve güvenliği hakkında daha fazla bilgi için bkz: [Application Insights veri güvenliği](app-insights-data-retention-privacy.md).