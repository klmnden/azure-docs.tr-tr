---
title: Sorun giderme ipuçları Azure Search'te bilişsel arama için | Microsoft Docs
description: İpuçları ve bilişsel ayarlamak için sorun giderme işlem hatları Azure Search'te arama.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 15fc879958bfd886210a90239e0247c60fe231f9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="troubleshooting-tips-for-cognitive-search"></a>Bilişsel arama için sorun giderme ipuçları

Bu makalede, ipuçları ve püf noktaları bilişsel arama yetenekleri Azure Search ile çalışmaya başlama gibi hareket tutmak için listesini içerir. 

Henüz yapmadıysanız, adım adım [Öğreticisi: bilişsel arama API çağrısı öğrenin](cognitive-search-quickstart-blob.md) uygulamada bilişsel arama enrichments blob veri kaynağına uygulamak için.

## <a name="tip-1-start-with-a-small-dataset"></a>İpucu 1: küçük bir veri kümesi ile Başlat
Sorunları hızla bulmak için en iyi yolu, sorunları gidermek hızlı artırmaktır. Dizin oluşturma süresini azaltmak için en iyi dizine alınacak belgeler sayısını azaltarak yoludur. 

Yalnızca belgeleri/kayıtları sayıda ile bir veri kaynağı oluşturarak başlayın. Belge örneğinizi dizinlenir belgeleri çeşitli iyi bir gösterimi olmalıdır. 

Uçtan uca ardışık düzen üzerinden belge örneğinizi çalıştırmak ve sonuçları gereksinimlerinizi karşılayacak denetleyin. Sonuçlardan memnun olduktan sonra veri kaynağınızı daha fazla dosya ekleyebilirsiniz.

## <a name="tip-2-make-sure-your-data-source-credentials-are-correct"></a>İpucu 2: veri kaynağı kimlik bilgilerinizi doğru olduğundan emin olun
Kullandığı bir dizin oluşturucuyu tanımlama kadar veri kaynağı bağlantısı doğrulanmaz. Dizin Oluşturucu veri alınamıyor söz herhangi bir hata görürseniz, emin olun:
- Bağlantı dizenizi doğrudur. Özel, SAS belirteci oluştururken, Azure Search tarafından beklenen biçimde kullandığınızdan emin olun. Bkz: [nasıl belirleneceğini bölüm kimlik bilgileri](
https://docs.microsoft.com/en-us/azure/search/search-howto-indexing-azure-blob-storage#how-to-specify-credentials) desteklenen farklı biçimleri hakkında bilgi edinmek için.
- Kapsayıcı adınızı dizin Oluşturucudaki doğrudur.

## <a name="tip-3-see-what-works-even-if-there-are-some-failures"></a>İpucu 3: bazı hatalar olsa bile ne çalıştığını görmek
Bazen küçük hata kendi parçaları bir dizin oluşturucu durdurur. Tek tek sorunlarını gidermek düşünüyorsanız uygundur. Ancak, hata, belirli bir tür yoksay ne akışları gerçekte çalıştığınız görebilmeniz için devam etmek dizin oluşturucu izin vermek isteyebilirsiniz.

Bu durumda, hatalarını yok saymak için dizin oluşturucu bildirmek isteyebilirsiniz. Ayarlayarak bunu *maxFailedItems* ve *maxFailedItemsPerBatch* -1 olarak dizin oluşturucu tanımının bir parçası olarak.

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
## <a name="tip-4-looking-at-enriched-documents-under-the-hood"></a>İpucu 4: başlık altında zenginleştirilmiş belgeleri bakarak. 
İyileştirmesini sırasında oluşturulan ve işlem tamamlandığında silinir geçici yapıları zenginleştirilmiş belgelerdir.

Dizin oluşturma sırasında oluşturulan zenginleştirilmiş belge görüntüsünü yakalamak için adlı bir alan eklemek ```enriched``` dizininiz için. Dizin Oluşturucu, bu belge için tüm enrichments dize gösterimini alanına otomatik olarak dökümünü yapar.

```enriched``` Alanı JSON bellek içi zenginleştirilmiş belgede mantıksal bir gösterimidir bir dize içerir.  Alan değeri geçerli bir JSON belgesi ancak kullanılır. Tırnak işaretleri kaçışlı değiştirmeniz gerekir böylece `\"` ile `"` biçimlendirilmiş JSON belgesi olarak görüntülemek için. 

Zenginleştirilmiş alan yalnızca, mantıksal şekli ifadeleri karşı değerlendirilen içeriğin anlamanıza yardımcı olması için hata ayıklama amacıyla tasarlanmıştır. Amacıyla dizin oluşturma için bu alanı bağlı olmaması gerekir.

Ekleme bir ```enriched``` alan hata ayıklama amacıyla, dizin tanımının bir parçası olarak:

#### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi
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

## <a name="tip-5-expected-content-fails-to-appear"></a>İpucu 5: görünmesi beklenen içerik başarısız

Eksik içerik dizin oluşturma sırasında kesiliyor belgeleri sonucu olabilir. Ücretsiz ve temel Katmanlar belge boyutuna düşük sınırlara sahiptir. Sınırı aşan herhangi bir dosyayı dizin oluşturma sırasında atıldı. Azure portalında bırakılan belgeler için kontrol edebilirsiniz. Arama hizmet panosunda, dizin oluşturucu Döşe çift tıklayın. Dizine başarılı belgeleri oranını gözden geçirin. % 100 değilse, daha fazla ayrıntı almak için oranı tıklatabilirsiniz. 

Sorun dosya boyutunu ilişkiliyse, bu gibi bir hata görebilirsiniz: "Blob < dosya adı >" Belge ayıklama geçerli hizmet katmanı için en fazla boyutu aşıyor < dosya boyutu > bayt boyutuna sahip. " Dizin Oluşturucu sınırları hakkında daha fazla bilgi için bkz: [hizmet sınırları](search-limits-quotas-capacity.md).

İkinci bir nedenle görünmesi başarısız olan içerik için ilgili giriş/çıkış eşleme hataları olabilir. Örneğin, bir çıktı hedef adı "Kişiler" ancak dizin alanın adı küçük "Kişiler". Aslında bir alanı boş olduğunda dizin oluşturma, başarılı bir şekilde düşündüğünüz şekilde sistem tüm ardışık düzeni için 201 başarı iletileri döndürebilirsiniz. 

## <a name="tip-6-extend-processing-beyond-maximum-run-time-24-hour-window"></a>İpucu 6: işlem en fazla çalışma süresi (24 saatlik bir aralık) ötesine genişletmek

Görüntü analiz hesaplama yoğunluklu bile basit durumlarda, bu nedenle görüntüleri özellikle büyük veya karmaşık olduğunda işleme sürelerini aşan izin verilen en fazla süre. 

Maksimum Çalıştırma süresi katmanı tarafından değişir: 24 saatlik Faturalanabilir katmanda bulunan dizin birkaç dakikada ücretsiz katmanı. İsteğe bağlı işleme için 24 saatlik süre içinde tamamlanması işlem başarısız olursa, kaldığı yerden işlemeyi çekme dizin oluşturucu için bir zamanlama geçin. 

Zamanlanmış dizin oluşturucular için dizin oluşturma en son bilinen iyi belge zamanlamaya göre devam ettirir. Dizin Oluşturucu, yinelenen bir zamanlama kullanarak, bir dizi saat veya tüm atanmamış işlenen görüntüleri işlenene kadar gün içinde görüntü biriktirme kendi aşamalardaki çalışabilir. Zamanlama sözdizimi hakkında daha fazla bilgi için bkz: [adım 3: Oluştur-bir-dizin oluşturucu](search-howto-indexing-azure-blob-storage.md#step-3-create-an-indexer).

Portal tabanlı (açıklandığı gibi Hızlı Başlangıç) dizin oluşturma işlemi için 1 saat olarak işleme sınırları seçeneği "bir kez çalıştır" dizin oluşturucusunu seçme (`"maxRunTime": "PT1H"`). Uzun bir şey işleme penceresi genişletmek isteyebilirsiniz.

## <a name="tip-7-increase-indexing-throughput"></a>İpucu 7: dizin oluşturma performansı artırma

İçin [paralel dizin](search-howto-reindex.md#parallel-indexing), birden çok kapsayıcı ya da aynı kapsayıcı içinde birden çok sanal klasörler verilerinizi yerleştirin. Daha sonra birden çok veri kaynağı ve dizin oluşturucu çiftleri oluşturun. Tüm Dizin oluşturucuların aynı skillset ve arama uygulamanız bu bölümlendirme farkında olması gerekmez şekilde aynı hedef arama dizine yazma kullanabilirsiniz.
Daha fazla bilgi için bkz: [dizin büyük veri kümeleri](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets).

## <a name="see-also"></a>Ayrıca bkz.
+ [Hızlı Başlangıç: portalda bilişsel arama işlem hattı oluşturma](cognitive-search-quickstart-blob.md)
+ [Öğretici: bilişsel arama REST API'leri öğrenin](cognitive-search-tutorial-blob.md)
+ [Veri kaynağı kimlik bilgilerini belirtme](search-howto-indexing-azure-blob-storage.md#how-to-specify-credentials)
+ [Dizin oluşturma büyük veri kümeleri](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
+ [Bir dizine zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)