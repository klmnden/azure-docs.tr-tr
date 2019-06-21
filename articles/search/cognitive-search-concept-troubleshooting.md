---
title: Bilişsel arama - Azure Search için sorun giderme ipuçları
description: İşlem hatları Azure Search'te arama, ipuçları ve bilişsel ayarlama işlemleri için sorun giderme.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 02/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: c0de4d2b9ad0d009b9cd363d19a2de3f29d810d4
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303463"
---
# <a name="troubleshooting-tips-for-cognitive-search"></a>Bilişsel arama için sorun giderme ipuçları

Bu makalede, ipuçları ve püf noktaları bilişsel arama özellikleri Azure Search kullanmaya başlama gibi hareket tutmak listesini içerir. 

Henüz yapmadıysanız, adım adım [Öğreticisi: Bilişsel arama API'leri çağırmak nasıl öğrenin](cognitive-search-quickstart-blob.md) uygulamada bilişsel arama zenginleştirmelerinin bir blob veri kaynağına uygulamak için.

## <a name="tip-1-start-with-a-small-dataset"></a>1 İpucu: Küçük bir veri kümesiyle çalışmaya başlayın
Sorunları hızlı bir şekilde bulmak için en iyi yolu sorunları düzeltebilir miyim hızını artırmaktır. Dizin oluşturma süresini azaltmak için en iyi belgelere sıralanacak sayısını azaltarak yoludur. 

Bir veri kaynağı yalnızca birkaç belge/ile oluşturmaya başlayın. Belge örneğinizi dizinlenir belgeleri çeşitli iyi bir temsili olmalıdır. 

Belge örneğinizi uçtan uca işlem hattı çalıştırma ve sonuçları ihtiyaçlarınızı karşılayacak denetleyin. Sonuçlardan memnun olduğunuzda, veri kaynağınıza başka dosyalar ekleyebilirsiniz.

## <a name="tip-2-make-sure-your-data-source-credentials-are-correct"></a>İpucu 2: Veri kaynağı kimlik bilgilerinizi doğru olduğundan emin olun
Veri kaynağı bağlantısı kullanan bir dizin oluşturucu tanımladığınız kadar doğrulanmaz. Dizin Oluşturucu verileri alınamıyor bahseden herhangi bir hata görürseniz emin olun:
- Bağlantı dizenizi doğrudur. Özel, SAS belirteçleri oluştururken, Azure Search tarafından beklenen biçimde kullandığınızdan emin olun. Bkz: [nasıl belirtileceğini bölüm kimlik bilgileri](
https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#how-to-specify-credentials) desteklenen farklı biçimleri hakkında bilgi edinmek için.
- Kapsayıcı adınız dizin Oluşturucudaki doğrudur.

## <a name="tip-3-see-what-works-even-if-there-are-some-failures"></a>İpucu 3: Bazı hatalar olsa bile neyin işe yaradığını görmek
Bazen bir dizin oluşturucu, parçalardaki küçük hata durdurur. Bu, tek tek sorunlarını düzeltmek düşünüyorsanız uygundur. Ancak, hatanın belirli bir tür yok saymak ne akışlar gerçekten çalıştığınız görebilmeniz için devam etmek dizin oluşturucuyu izin vermek isteyebilirsiniz.

Bu durumda, hatalarını yok saymak için dizin oluşturucuyu bildirmek isteyebilirsiniz. Ayarlayarak bunu *maxFailedItems* ve *maxFailedItemsPerBatch* -1 olarak dizin oluşturucu tanımı bir parçası olarak.

```
{
  "// rest of your indexer definition
   "parameters":
   {
      "maxFailedItems":-1,
      "maxFailedItemsPerBatch":-1
   }
}
```
## <a name="tip-4-looking-at-enriched-documents-under-the-hood"></a>İpucu 4: Başlık altında zenginleştirilmiş belgelere bakarak 
Zenginleştirme sırasında oluşturulan ve işlenmesi tamamlandığında silinir geçici yapıları zenginleştirilmiş belgelerdir.

Dizin oluşturma sırasında oluşturulan zenginleştirilmiş belgenin anlık görüntüsünü yakalamak için dizininize ```enriched``` adlı bir alan ekleyin. Dizin oluşturucu, otomatik olarak alana, o belgenin tüm zenginleştirmelerinin dize gösteriminin dökümünü alır.

```enriched``` alanı, JSON’da bellek içi zenginleştirilmiş belgenin mantıksal gösterimi olan bir dize içerir.  Ancak alan değeri geçerli bir JSON belgesidir. Tırnak işaretlerine kaçış karakteri eklenir, böylece belgeyi biçimlendirilmiş JSON olarak görüntülemek için `\"` öğesini `"` ile değiştirmeniz gerekir. 

Zenginleştirilmiş alan, yalnızca mantıksal ifadeleri karşı değerlendirilmekte içeriği şeklini anlamanıza yardımcı olması için hata ayıklama amacıyla tasarlanmıştır. Bu alan için dizin oluşturma amacıyla bağlı olmamalıdır.

Ekleme bir ```enriched``` dizin tanımınız, hata ayıklama amacıyla bir parçası olarak alan:

#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
```json
{
  "fields": [
    // other fields go here.
    {
      "name": "enriched",
      "type": "Edm.String",
      "searchable": false,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```

## <a name="tip-5-expected-content-fails-to-appear"></a>İpucu 5: Görüntülenecek içerik başarısız bekleniyor

Eksik içerik dizin oluşturma sırasında bırakılmak belgeleri sonucu olabilir. Ücretsiz ve temel katmanlarında belge boyutuna düşük sınırlara sahiptir. Sınırı aşan herhangi bir dosya, dizin oluşturma sırasında bırakılır. Bırakılan belgeleri Azure portalında denetleyebilirsiniz. Arama hizmet panosunda, dizin oluşturucu kutucuğu çift tıklayın. Dizini oluşturulmuş başarılı belgelerini oranı gözden geçirin. % 100 değilse, daha fazla ayrıntı almak için oran tıklayabilirsiniz. 

Sorun için dosya boyutu ile ilgiliyse, bu gibi bir hata görebilirsiniz: "Blob \<dosya adı >" boyutunun \<dosya boyutu > bayt cinsinden geçerli hizmet katmanı için belge ayıklama boyutu üst sınırı aşıyor. " Dizin Oluşturucu sınırları hakkında daha fazla bilgi için bkz. [hizmet sınırları](search-limits-quotas-capacity.md).

Görüntülenecek başarısız olan içerik için ikinci bir nedeni, ilgili giriş/çıkış eşleme hataları olabilir. Örneğin, "Kişiler" çıkış hedef adıdır ancak küçük harf "Kişiler" dizin alan adıdır. Aslında bir alan boş olduğunda dizin oluşturma, başarılı anlattık sistem işlem hattının tamamı için 201 Başarı iletilerinin döndürebilir. 

## <a name="tip-6-extend-processing-beyond-maximum-run-time-24-hour-window"></a>İpucu 6: İşleme en fazla çalışma süresi (24 saatlik pencere) ötesine genişletir

Görüntü analizi işlem bakımından yoğun bile basit durumlarda, bu nedenle görüntüleri özellikle büyük veya karmaşık olduğunda işleme sürelerini aşan izin verilen en fazla süreyi. 

Maksimum Çalıştırma süresi katmana göre değişiklik gösterir: 24 saatlik Faturalanabilir katmanlarda dizin oluşturma birkaç dakika ücretsiz katmanı. Talep üzerine işleme için bir 24 saatlik süre içinde tamamlanması işleme başarısız olursa, kaldığı işlemeyi çekme dizin oluşturucu için bir zamanlama geçin. 

Zamanlanmış dizin oluşturucular için dizin oluşturma en son bilinen iyi belge zamanlamaya göre devam ettirir. Dizin Oluşturucu, yinelenen bir zamanlama kullanarak saatler veya günler, hazırlanmamış işlenen tüm görüntüleri işlenene kadar bir dizi üzerinden görüntü biriktirme listesi aracılığıyla kendi şekilde çalışabilirsiniz. Zamanlama sözdizimi hakkında daha fazla bilgi için bkz. [3. adım: Bir-dizin oluşturucu oluşturma](search-howto-indexing-azure-blob-storage.md#step-3-create-an-indexer) veya [dizin oluşturucular için Azure Search zamanlama](search-howto-schedule-indexers.md).

> [!NOTE]
> Bir dizin oluşturucu, belirli bir zamanlama için ayarlanır ancak tekrar tekrar aynı başarısız tekrar tekrar her zaman bu çalıştırmaları belge, dizin oluşturucu başlar (en fazla 24 saatte bir en az bir kez en fazla) daha az sıklıkta aralıkta kadar başarılı bir şekilde çalıştırma ilerleme aga hale getirir .  Sabit ne olursa olsun, belirli bir noktada takılmış için dizin oluşturucuyu neden olan sorunu düşünüyorsanız, üzerinde isteğe bağlı bir dizin oluşturucu yürütülmesi gerçekleştirebilir ve ilgili başarıyla, ilerleme, dizin oluşturucu döndürür, kümesi zamanlama aralığı için yeniden.

Portal tabanlı (hızlı başlangıç açıklandığı şekilde) dizin oluşturma için 1 saate işleme sınırları seçenek "bir kez çalıştır" Dizin Oluşturucusu seçme (`"maxRunTime": "PT1H"`). Uzun bir şey işleme penceresini genişletmek isteyebilirsiniz.

## <a name="tip-7-increase-indexing-throughput"></a>7 ipucu: Dizin oluşturma performansı artırma

İçin [paralel dizin](search-howto-large-index.md), verilerinizi birden çok kapsayıcı ya da aynı kapsayıcı içinde birden çok sanal klasör yerleştirin. Daha sonra birden çok veri kaynağı ve dizin oluşturucu çifti oluşturun. Tüm Dizin oluşturucuların aynı becerilerine ve arama uygulamanızı bu bölümlere farkında olması gerekmez, aynı hedef search dizinine yazma kullanabilirsiniz.
Daha fazla bilgi için [dizin oluşturma, büyük veri kümelerini](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets).

## <a name="see-also"></a>Ayrıca bkz.
+ [Hızlı Başlangıç: Portalda bir bilişsel arama işlem hattı oluşturma](cognitive-search-quickstart-blob.md)
+ [Öğretici: Bilişsel arama REST API'leri öğrenin](cognitive-search-tutorial-blob.md)
+ [Veri kaynağı kimlik bilgilerini belirtme](search-howto-indexing-azure-blob-storage.md#how-to-specify-credentials)
+ [Büyük veri kümelerini dizin oluşturma](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Nasıl bir dizin zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
