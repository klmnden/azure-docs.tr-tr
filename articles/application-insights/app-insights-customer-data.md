---
title: Azure Application Insights'ta depolanan kişisel veriler için Kılavuzu | Microsoft Docs
description: Bu makalede, kişisel verileri tanımlamak ve kaldırmak için Azure Application Insights ve yöntemleri depolanan yönetmek açıklar.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: Evgeny.Ternovsky;mbullwin
ms.openlocfilehash: 1f9f7d53fc09111e0060c934e3869326fcaadb7e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655363"
---
# <a name="guidance-for-personal-data-stored-in-application-insights"></a>Application Insights'ta depolanan kişisel veriler için yönergeler

Application Insights kişisel veriler bulunma olasılığı olduğu bir veri deposu olur. Bu makalede, Application Insights'ta bu verileri genellikle, bu verileri işlemek için özellikleri yanı sıra bulunduğu açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Kişisel veri işleme için stratejisi

Bu, şirketinizin en fazla ve sonuçta ile özel işleyecek stratejisini belirlemek için veriler (varsa) olacaktır olsa da, bazı olası yaklaşım verilmiştir. Bir teknik açısından çoğu gelen tercih sırasına göre listelenen az tercih için:
* Mümkünse, durdurmak koleksiyonunu belirsizleştirirseniz, Anonimleştir veya aksi halde olarak kabul edilmeden dışlanacak toplanmakta olan verileri ayarlamak _özel_. Bu yöntem stratejisi işleme pahalı ve etkili bir veri oluşturmak için gereken kaydetme tercih edilen yaklaşım kullanılır.
* Bir veri platformu ve performans üzerindeki etkisini azaltmak için verileri normalleştirmek değil, mümkün olduğunda, çalışır. Örneğin, açık bir kullanıcı kimliği günlüğü yerine kullanıcıadı ilişkilendirmek veri için bir arama ve ardından başka bir yerde günlüğe kaydedilebilir bir iç kimlik ayrıntıları oluşturun. Kullanıcılarınızdan biri, kendi kişisel bilgilerini silmek için sorun varsa bu şekilde, yalnızca kullanıcıya karşılık gelen arama tablosunda satırın silinmesi yeterli olması mümkündür. 
* Son olarak, özel veri toplanması gereken, temizleme API yol ve dışa aktarma ve bir kullanıcıyla ilişkili herhangi bir özel veri silme geçici olabilir yükümlülüklerin karşılamak için varolan sorgu API yol geçici bir işlem oluşturun.

## <a name="where-to-look-for-private-data-in-application-insights"></a>Application Insights özel veri arayın nerede?

Application Insights, verilerinizi şemaya prescribing sırasında özel değerlere sahip her bir alan geçersiz kılma olanak tanıyan bir tamamen esnek deposu olur. Bu nedenle, tam olarak burada özel veri belirli uygulamanızda bulunacaktır söylemek mümkün değildir. Aşağıdaki konumlardan ancak iyi envanterinize başlangıç noktaları:

* *IP adreslerini*: sırasında Application Insights varsayılan olarak belirsizleştirirseniz "0.0.0.0" için tüm IP adresi alanlarını, oturum bilgilerini korumak için gerçek kullanıcı IP bu değerle geçersiz kılmak için oldukça genel bir desen. Son 24 saat boyunca "0.0.0.0" dışında IP adresi sütundaki değerleri içeren herhangi bir tabloyu bulmak için aşağıdaki Analytics sorgu kullanılabilir:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *Kullanıcı kimliklerini*: varsayılan olarak, Application Insights rastgele oluşturulmuş kimlikleri kullanıcı ve oturum izleme için kullanır. Ancak, uygulama için daha uygun bir kimlik depolamak için geçersiz kılındı bu alanları görmek için yaygındır. Örneğin: kullanıcı adları, AAD GUID'ler, vs. Bu kimlikleri genellikle olduğu kabul edilir kapsam içinde kişisel veri ve bu nedenle, uygun şekilde yapılması gerekir. Bizim önerimiz her zaman belirsizleştirirseniz veya bu kimlikleri Anonimleştir çalışmaktır. Bu değerleri yaygın olarak burada bulunan alanlar session_Id, USER_ID, user_AuthenticatedId, user_AccountId yanı sıra customDimensions içerir.
* *Özel veri*: Application Insights herhangi bir veri türü için bir dizi özel boyut append olanak tanır. Bu boyutlar olabilir *herhangi* veri. Son 24 saat boyunca toplanan herhangi bir özel boyut belirlemek için aşağıdaki sorguyu kullanın:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Bellek içi ve aktarım sırasında verileri*: Application Insights özel durumlar, istekleri, bağımlılık çağrıları ve izlemeleri izleyecek. Özel veri genellikle kod ve HTTP çağrısı düzeyinde toplanabilir. Özel durumlar, istekleri, bağımlılıklar ve izlemeleri tabloları tür verileri tanımlamak için gözden geçirin. Kullanım [telemetri başlatıcıları](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) mümkün olduğunda bu verileri belirsizleştirirseniz.
* *Hata ayıklayıcı yakalamaları anlık görüntü*: [anlık görüntü hata ayıklayıcı](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) Application Insights özelliği, uygulamanızı üretim örneğinde bir özel durum yakalandı her hata ayıklama anlık görüntüleri toplamak olanak sağlar. Anlık görüntüler, yığındaki her adım, yerel değişkenleri için değerleri yanı sıra özel durumları başta tam yığın izlemesi açığa çıkarır. Ne yazık ki, bu özellik ek noktaları ya da anlık görüntü verileri programlı olarak erişim seçmeli silme işlemi için izin vermiyor. Varsayılan anlık görüntü saklama oranı uyumluluk gereksinimlerini karşılamadığı varsa, bu nedenle, özelliği devre dışı bırakmak için önerilir.

## <a name="how-to-export-and-delete-private-data"></a>Dışarı aktarma ve özel verileri silme

Bu __kesinlikle__ önerilir, olası obfuscating veya, anonymizing özel veri koleksiyonunu devre dışı bırakmak için veri toplama İlkesi yapılandırılacak ya da aksi takdirde "özel" kabul kaldırmak için değiştirme Toplanan bırakıldı, sizin ve ekibinizin tanımlamak ve bir strateji otomatikleştirmek müşterilerinizin verilerine etkileşim için bir arabirim oluşturmak için maliyetleri ve devam eden bakım maliyetlerine neden olacak sonra verileri işleme. Ayrıca, Application Insights ve eş zamanlı sorgu büyük miktarda pkı'ya maliyetli olabilir veya temizleme API çağrıları Application Insights işlevselliği ile tüm diğer etkileşimi olumsuz olanağına sahip. Başka bir deyişle, burada özel veri gerekir toplanmasını geçerli gerçekten bazı senaryolar vardır. Bu durumlarda, bu bölümde açıklandığı gibi veri yapılmalıdır.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Görünüm ve dışarı aktarma

Her ikisi de görüntüleyebilir ve veri istekleri, dışarı aktarma için [sorgu API](https://dev.applicationinsights.io/quickstart) kullanılmalıdır. Kullanıcılarınıza sunmak için uygun bir veri şekli dönüştürmek için mantığı uygulamak size olacaktır. [Azure işlevleri](https://azure.microsoft.com/services/functions/) böyle bir mantık barındırmak için harika bir yerdir.

### <a name="delete"></a>Sil

> [!WARNING]
> Application ınsights'ta siler bozucu ve çevrilemez! Lütfen bunların yürütme dikkatli.

Gizlilik işleme parçası yazı olarak bir "Temizle" API yolu biz kullanılabilir yaptınız. Bu yol, böylece ile ilişkili riski nedeniyle olabildiğince az kullanılmalıdır olası performans etkisini ve tüm yukarı toplamalar, ölçümleri ve Application Insights verilerinizin diğer yönlerini eğme olma olasılığı. Bkz: [kişisel veri işleme için stratejisi](#strategy-for-personal-data-handling) yaklaşımlar özel verileri işlemek bölüm yukarıda.

Temizleme, hiçbir kullanıcı veya (bile kaynak sahibi dahil olmak üzere) açıkça alınmadan verilen Azure Kaynak Yöneticisi'ndeki rol yürütme iznine sahip olduğu yüksek ayrıcalıklı bir işlemdir. Bu rol _veri Purger_ ve olası veri kaybını nedeniyle dikkatle temsilci.

Azure Resource Manager rol atandıktan sonra iki yeni API yollar kullanılabilir, tam Geliştirici belgeleri olan ve bağlı şekil çağırın:

* [POST Temizleme](https://docs.microsoft.com/rest/api/application-insights/components/purge) - silmek için veri parametrelerini belirten nesnesini alır ve bir başvuru GUID döndürür
* GET Temizle durumu - POST temizleme çağrısı temizleme API'nizi durumunu belirlemek için arayabileceğiniz bir URL içeren bir 'x-ms-durum-location' üstbilgisi döndürür. Örneğin:
   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

Temizleme işlemleri büyük çoğunluğu ağır Application Insights tarafından kullanılan veri platformu etkilerini nedeniyle SLA çok hızlı tamamlarken resmi 30 günde SLA temizleme işlemlerinin tamamlanması için ayarlanır.

## <a name="next-steps"></a>Sonraki adımlar
Verileri nasıl toplanan, işlenen ve güvenliği hakkında daha fazla bilgi için bkz: [Application Insights veri güvenliği](app-insights-data-retention-privacy.md).