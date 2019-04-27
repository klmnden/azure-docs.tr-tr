---
title: Bir arama dizininin - Azure Search için Puanlama profilleri ekleyin
description: Azure Search arama sonuçları için arama derecelendirme puanlarını, Puanlama profillerini ekleyerek artırın.
ms.date: 01/31/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: eae7de00294a6a09cb7f942d11ee2391710fc55f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60844301"
---
# <a name="add-scoring-profiles-to-an-azure-search-index"></a>Azure Search dizini için Puanlama profilleri ekleyin

  Puanlama, hesaplama için başvuran bir *arama puanı* arama sonuçlarında çıkmadıysa her öğe için. Puan, bir öğenin ilgi geçerli arama işlemini bağlamında bir göstergesidir. Yüksek puanı, daha fazla ilgili öğe. Arama sonuçlarında, her öğe için hesaplanan arama puanları düşük, dayalı olarak yüksek sıralı bir sıralama öğelerdir.  

 Azure Search kullanan bir başlangıç puanı işlem Puanlama varsayılan ancak hesaplamayı özelleştirebileceğiniz bir *Puanlama profili*. Puanlama profilleri, arama sonuçlarında derecelendirme öğeleri üzerinde daha fazla denetim sağlar. Örneğin,, gelir potansiyellerini bağlı öğeleri artırın, yeni öğeleri yükseltmek veya belki uzun envanterinde öğelerini artırmak isteyebilirsiniz.  

 Puanlama profili ağırlıklı alanlar, İşlevler ve parametreleri oluşan dizin tanımını bir parçasıdır.  

 Puanlama profili benzer bir fikir vermek için aşağıdaki örnek 'coğrafi' adlı basit bir profili gösterir. Bu bir arama terimi olmayan öğeler artırıyor **hotelName** alan. Ayrıca kullanır `distance` geçerli konumun içinde on kilometre öğelerini tanınacağını işlevi. Birisi 'inn' terimini arar ve Otel adı bir parçası olarak 'inn' olmuyor, geçerli konum bir 10 KM yarıçapını içinde 'inn' ile hotels içeren belgeleri arama sonuçlarında daha yukarıda görünür.  


```  
"scoringProfiles": [
  {  
    "name":"geo",
    "text": {  
      "weights": {  
        "hotelName": 5
      }                              
    },
    "functions": [
      {  
        "type": "distance",
        "boost": 5,
        "fieldName": "location",
        "interpolation": "logarithmic",
        "distance": {
          "referencePointParameter": "currentLocation",
          "boostingDistance": 10
        }                        
      }                                      
    ]                     
  }            
]
```  


 Bu Puanlama profili kullanmak için sorgunuzu sorgu dizesinde profilini belirtmek için formüle. Aşağıdaki sorguda, sorgu parametresi fark `scoringProfile=geo` istek.  

```  
GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2017-11-11  
```  

 Bu sorgu, 'inn' terimini arar ve geçerli konumu geçirir. Bu sorgu gibi diğer parametreleri içerdiğini unutmayın `scoringParameter`. Sorgu parametreleri bölümünde açıklanmıştır [arama belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).  

 Tıklayın [örnek](#bkmk_ex) Puanlama profili daha ayrıntılı bir örneği gözden geçirmek için.  

## <a name="what-is-default-scoring"></a>Hangi varsayılan Puanlama olan?  
 Puanlama derece sıralı sonuç kümesinde her öğe için bir arama puanı hesaplar. Arama sonuç kümesi içindeki her bir öğe arama puanı atanan ve ardından en düşüğe. Daha yüksek puan öğeleriyle uygulamaya döndürülür. Varsayılan olarak, en çok 50 döndürülür, ancak kullanabileceğiniz `$top` parametresi (en fazla 1000 tek bir yanıt) öğeleri daha küçük ya da daha büyük bir sayısını döndürür.  

Arama puanı istatistiksel veriler ve sorgu özelliklerini göre hesaplanır. Azure arama, sorgu dizesinde arama terimlerini içeren belgeleri bulur (bazı veya tüm bağlı `searchMode`), birçok örneklerini arama terimini içeren belgeleri öncelik tanıdığından. Arama puanı terimi, ancak ortak belge içindeki veri topluluğunuza arasında nadir olması durumunda bile daha yüksek artar. Bu yaklaşım işlem ilgi temeli olarak bilinir [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) ya da terim sıklığı ters belge sıklığı.  

 Çağıran uygulama için döndürmeden var. özel sıralama varsayıldığında, sonuçları ardından arama puanı tarafından sıralanır. $Top belirtilmezse, en yüksek arama puanı sahip 50 öğeleri döndürülür.  

 Bir sonuç kümesi arama puanı değerleri yinelenebilir. Örneğin, 10 öğelerin 1.2, puanı olabilir bir puan ile 20 öğe 1.0 ve 0,5 puanı 20 öğe. Aynı puanlanmış öğelerin sıralamasını birden çok isabet aynı arama puanı varsa, tanımlı değildir ve tutarlı değil. Sorguyu yeniden ve görebileceği çalışma öğelerini kaydırma konumu. İki özdeş bir puan öğeleriyle göz önünde bulundurulduğunda, hangisinin ilk olarak görünür bir garanti yoktur.  

## <a name="when-to-use-custom-scoring"></a>Ne zaman özel puanlamaya kullanılır?  
 Varsayılan davranış sıralaması toplantı içinde kadar yeterli iş hedeflerinize olduğunuzda değil, bir veya daha fazla Puanlama profilleri oluşturmanız gerekir. Örneğin, aramanın ilgi düzeyini yeni eklenen öğeleri favor karar verebilirsiniz. Benzer şekilde, kar marjı içeren bir alan veya olası gelir belirten başka bir alan olabilir. İşletmenize avantaj getiren isabet artırma Puanlama profilleri kullanmaya karar vermeden önemli bir etken olabilir.  

 İlgi düzeyini tabanlı sıralama Puanlama profilleri de uygulanır. Arama sonuçları sayfası, fiyat, tarih, sıralama veya ilgi sıralamanıza olanak tanıyan geçmişte kullandığınız göz önünde bulundurun. Azure Search'te, Puanlama profillerini 'ilgi' seçeneği sürücü. İlgi tanımı, iş hedeflerini ve arama deneyimi sunmak istediğiniz türünü predicated denetlenir.  

##  <a name="bkmk_ex"></a> Örnek  
 Daha önce belirtildiği gibi özelleştirilmiş Puanlama bir dizin şemasında tanımlanan bir veya daha fazla Puanlama profilleri aracılığıyla uygulanır.  

 Bu örnek, iki Puanlama profilleri olan bir dizin şemasını gösterir (`boostGenre`, `newAndHighlyRated`). Herhangi bir sorgu karşı bir sorgu parametresi profili sonuç kümesi puanlamak için kullanacağınız her iki profil içeren bu dizini.  

```  
{  
  "name": "musicstoreindex",  
  "fields": [  
    { "name": "key", "type": "Edm.String", "key": true },  
    { "name": "albumTitle", "type": "Edm.String" },  
    { "name": "albumUrl", "type": "Edm.String", "filterable": false },  
    { "name": "genre", "type": "Edm.String" },  
    { "name": "genreDescription", "type": "Edm.String", "filterable": false },  
    { "name": "artistName", "type": "Edm.String" },  
    { "name": "orderableOnline", "type": "Edm.Boolean" },  
    { "name": "rating", "type": "Edm.Int32" },  
    { "name": "tags", "type": "Collection(Edm.String)" },  
    { "name": "price", "type": "Edm.Double", "filterable": false },  
    { "name": "margin", "type": "Edm.Int32", "retrievable": false },  
    { "name": "inventory", "type": "Edm.Int32" },  
    { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }  
  ],  
  "scoringProfiles": [  
    {  
      "name": "boostGenre",  
      "text": {  
        "weights": {  
          "albumTitle": 1.5,  
          "genre": 5,  
          "artistName": 2  
        }  
      }  
    },  
    {  
      "name": "newAndHighlyRated",  
      "functions": [  
        {  
          "type": "freshness",  
          "fieldName": "lastUpdated",  
          "boost": 10,  
          "interpolation": "quadratic",  
          "freshness": {  
            "boostingDuration": "P365D"  
          }  
        },  
        {
          "type": "magnitude",  
          "fieldName": "rating",  
          "boost": 10,  
          "interpolation": "linear",  
          "magnitude": {  
            "boostingRangeStart": 1,  
            "boostingRangeEnd": 5,  
            "constantBoostBeyondRange": false  
          }  
        }  
      ]  
    }  
  ],  
  "suggesters": [  
    {  
      "name": "sg",  
      "searchMode": "analyzingInfixMatching",  
      "sourceFields": [ "albumTitle", "artistName" ]  
    }  
  ]   
}  
```  

## <a name="workflow"></a>İş akışı  
 Özel Puanlama davranışı uygulamak için dizini tanımlayan şemayı Puanlama profili ekleyin. En fazla 100 Puanlama profilleri dizin içinde olabilir (bakın [hizmet sınırları](search-limits-quotas-capacity.md)), ancak yalnızca belirli bir sorgu zamanında bir profil belirtebilirsiniz.  

 İle başlayan [şablon](#bkmk_template) bu konuda sağlanan.  

 Bir ad sağlayın. Puanlama profilleri isteğe bağlıdır, ancak bir eklerseniz, adı gereklidir. Alanlar için adlandırma kurallarını takip ettiğinizden emin olun (özel karakterler ve ayrılmış sözcükler engelleyen bir harf ile başlayan). Bkz: [adlandırma kuralları &#40;Azure Search&#41; ](https://docs.microsoft.com/rest/api/searchservice/naming-rules) tam listesi için.  

 Puanlama profili gövdesi ağırlıklı alanları ve işlevleri oluşturulur.  

|||  
|-|-|  
|**Ağırlıkları**|Bir alan göreli ağırlık atayın ad-değer çiftlerini belirtin. İçinde [örnek](#bkmk_ex), albumTitle, türe ve artistName artırmalı 1.5, 5 ve 2 sırasıyla alanlardır. Neden Tarz diğerlerinden çok daha yüksek boosted? Arama bir biraz homojen olan veriler üzerinde yürütülür ('tarzı' de olduğu gibi `musicstoreindex`), göreli ağırlıkları daha büyük bir varyans gerekebilir. Örneğin, `musicstoreindex`, 'rock' hem bir türe ve aynı şekilde tümcecik oluşturulmuş Tarz açıklamalarında görünür. Tarz açıklama gölgede için Tarz istiyorsanız, çok daha yüksek göreli ağırlık Tarz alan gerekir.|  
|**İşlevler**|Ek hesaplamalar için belirli bağlamlarda gerekli olduğunda kullanılır. Geçerli değerler `freshness`, `magnitude`, `distance`, ve `tag`. Her işlev için benzersiz olan parametre yok.<br /><br /> -   `freshness` artırmak istediğiniz zaman kullanılması gerektiğini nasıl yeni veya eski bir öğe tarafından. Bu işlev yalnızca ile kullanılabilir `datetime` alanlar (edm. DataTimeOffset). Not `boostingDuration` öznitelik kullanılır yalnızca `freshness` işlevi.<br />-   `magnitude` nasıl yüksek göre artırmak istiyorsanız veya sayısal bir değer düşükse olduğunda kullanılmalıdır. Kar marjı, en yüksek fiyat, düşük fiyat veya indirme işlemlerinin sayısını artırmak için bu işlevi çağırın senaryoları içerir. Bu işlev, yalnızca çift ve tamsayı alanları ile kullanılabilir.<br />     İçin `magnitude` işlevi ters aralığı düşük için yüksek ters deseni (örneğin, düşük fiyatlı boost öğeleri öğelerden Otomobille daha yüksek daha fazla) istiyorsanız. Bir dizi fiyatları $100'den 1'e göz önünde bulundurulduğunda, ayarlarsınız `boostingRangeStart` 100 ve `boostingRangeEnd` düşük fiyatlı öğeleri artırmak için 1.<br />-   `distance` Yakınlık veya coğrafi konum artırmak istediğiniz zaman kullanılmalıdır. Bu işlev yalnızca ile kullanılabilir `Edm.GeographyPoint` alanları.<br />-   `tag` Ortak Belgeler ve arama sorguları arasında etiketlere göre artırmak istediğiniz zaman kullanılmalıdır. Bu işlev yalnızca ile kullanılabilir `Edm.String` ve `Collection(Edm.String)` alanları.<br /><br /> **İşlevlerini kullanmak için kurallar**<br /><br /> İşlev türü (`freshness`, `magnitude`, `distance`), `tag` küçük harf olması gerekir.<br /><br /> İşlevler, null veya boş değerler içeremez. Özellikle, fieldname eklerseniz, bir şeyler ayarlamak zorunda.<br /><br /> İşlevleri yalnızca filtrelenebilir alanlardan için uygulanabilir. Bkz: [Create Index &#40;Azure arama hizmeti REST API'si&#41; ](https://docs.microsoft.com/rest/api/searchservice/create-index) filtrelenebilir alanlar hakkında daha fazla bilgi için.<br /><br /> İşlevleri, bir dizin alanları koleksiyonu içinde tanımlanan alanlara yalnızca uygulanabilir.|  

 Dizin tanımlandıktan sonra belgelerin ve ardından dizin şeması yükleyerek dizini oluşturun. Bkz: [Create Index &#40;Azure arama hizmeti REST API'si&#41; ](https://docs.microsoft.com/rest/api/searchservice/create-index) ve [ekleme, güncelleştirme veya silme belgeleri &#40;Azure arama hizmeti REST API'si&#41; ](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bu işlemler hakkında yönergeler için. Dizin oluşturulduktan sonra arama verilerinizle birlikte çalışan bir işlev Puanlama profili olması gerekir.  

##  <a name="bkmk_template"></a> Şablonu  
 Bu bölümde, söz dizimi ve puanlama profilleri şablonu gösterilmektedir. Başvurmak [dizin öznitelikleri başvurusu](#bkmk_indexref) öznitelikler açıklaması için sonraki bölümde.  

```  
. . .   
"scoringProfiles": [  
  {   
    "name": "name of scoring profile",   
    "text": (optional, only applies to searchable fields) {   
      "weights": {   
        "searchable_field_name": relative_weight_value (positive #'s),   
        ...   
      }   
    },   
    "functions": (optional) [  
      {   
        "type": "magnitude | freshness | distance | tag",   
        "boost": # (positive number used as multiplier for raw score != 1),   
        "fieldName": "...",   
        "interpolation": "constant | linear (default) | quadratic | logarithmic",   

        "magnitude": {
          "boostingRangeStart": #,   
          "boostingRangeEnd": #,   
          "constantBoostBeyondRange": true | false (default)
        }  

        // ( - or -)  

        "freshness": {
          "boostingDuration": "..." (value representing timespan over which boosting occurs)   
        }  

        // ( - or -)  

        "distance": {
          "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)   
          "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)   
        }   

        // ( - or -)  

        "tag": {
          "tagsParameter":  "..."(parameter to be passed in queries to specify a list of tags to compare against target field)   
        }
      }
    ],   
    "functionAggregation": (optional, applies only when functions are specified) "sum (default) | average | minimum | maximum | firstMatching"   
  }   
],   
"defaultScoringProfile": (optional) "...",   
. . .  
```  

##  <a name="bkmk_indexref"></a> Dizin öznitelikleri başvurusu  

> [!NOTE]  
>  Puanlama işlevi yalnızca filtrelenebilir alanlarına uygulanabilir.  

|Öznitelik|Açıklama|  
|---------------|-----------------|  
|`Name`|Gereklidir. Puanlama profili adıdır. Bu, bir alanın aynı adlandırma kurallarını izler. Bir harf ile başlamalı, nokta, iki nokta üst üste içeremez veya semboller, ' @ 'Azure Search' (büyük-küçük harfe duyarlı) deyim ile başlayamaz.|  
|`Text`|Ağırlıklar özelliği içerir.|  
|`Weights`|İsteğe bağlı. Bir alan adı ve göreli ağırlığı olarak belirten bir ad-değer çifti. Göreli ağırlık bir pozitif tamsayı veya kayan noktalı sayı olmalıdır. En yüksek değer Int32 ' dir. MaxValue.<br /><br /> Alan adına karşılık gelen ağırlık olmadan belirtebilirsiniz. Ağırlıklara göre başka bir alan önemini belirtmek için kullanılır.|  
|`Functions`|İsteğe bağlı. Puanlama işlevi yalnızca filtrelenebilir alanlara uygulanabilir unutmayın.|  
|`Type`|Puanlama işlevleri için gereklidir. Kullanılacak işlev türünü belirtir. Geçerli değerler, büyüklük, yenilik, uzaklığı ve etiket içerir. Her Puanlama profili içinde birden fazla işlev içerebilir. İşlev adı, küçük harf olması gerekir.|  
|`Boost`|Puanlama işlevleri için gereklidir. Ham puan çarpanı olarak kullanılan pozitif bir sayı. 1'e eşit olamaz.|  
|`Fieldname`|Puanlama işlevleri için gereklidir. Puanlama işlevi yalnızca dizinin alan koleksiyonunun parçası olan ve filtrelenebilir olan alanlar için uygulanabilir. Ayrıca, her işlev türü (güncellik datetime alanları integer veya double alanları ile azalır ve uzaklık konumunu alanlarla kullanılır) ek kısıtlamalar ortaya çıkarır. Yalnızca işlev tanımı başına tek bir alan belirtebilirsiniz. Örneğin, büyüklük iki kez aynı profilinde kullanmak için her bir alan için bir tane iki tanımları büyüklük dahil gerekecektir.|  
|`Interpolation`|Puanlama işlevleri için gereklidir. Eğimi, aralığın başından aralığın sonuna bir artış artırma tanımlar. Geçerli değerler, doğrusal (varsayılan), sabit, dereceden ve Logaritmik içerir. Bkz: [ayarlamak ilişkilendirme](#bkmk_interpolation) Ayrıntılar için.|  
|`magnitude`|Büyüklük Puanlama işlevi, sayısal bir alan için değer aralığını göre sıralamasına değiştirmek için kullanılır. Bunun en yaygın kullanım örnekleri bazıları şunlardır:<br /><br /> -   **Yıldız değerlendirmelerinde:** Puanlama "Yıldız derecesi" alandaki değere göre değiştirin. Daha yüksek derecelendirme öğeyle ilk iki öğeyi ilgili olduğunda görüntülenir.<br />-   **Kenar boşluğu:** İki belge uygun olduğunda, bir satıcıya daha yüksek bir kenar boşlukları içeren belgelerin artırmak isteyebilirsiniz.<br />-   **Sayıları'na tıklayın:** Ürünleri ya da sayfalarına eylemleri aracılığıyla izleyen Uygulamalar'a tıklayın için en çok trafiği almak için eğilimli boost öğelerine büyüklük kullanabilirsiniz.<br />-   **Sayıları indirin:** İzleme yüklemeleri, büyüklük işlevi sağlar, artırma öğelerini, uygulamalar için en çok indirilenler vardır.|  
|`magnitude` &#124; `boostingRangeStart`|Üzerinde büyüklüğün puanlandığı aralığın başlangıç değerini ayarlar. Değer bir tamsayı veya kayan noktalı sayı olmalıdır. 1 ile 4 arasındaki yıldız değerlendirmelerinde için bu 1 olur. % 50 üzerindeki kenar boşlukları için bu 50 olur.|  
|`magnitude` &#124; `boostingRangeEnd`|Üzerinde büyüklüğün puanlandığı aralığın bitiş değerini ayarlar. Değer bir tamsayı veya kayan noktalı sayı olmalıdır. 1 ile 4 arasındaki yıldız değerlendirmelerinde için bu 4 olur.|  
|`magnitude` &#124; `constantBoostBeyondRange`|Geçerli değerler true veya false (varsayılan) olmalı. Ayarlandığında true, tam artırma aralığının üst son'dan büyük hedef alan için bir değere sahip belgelere uygulanmaya devam edecek. False ise, bu işlevin artırma özelliği hedef alanın aralığın dışında bir değere sahip belgelere uygulanmaz.|  
|`freshness`|Puanlama işlevi güncellik değerlere göre öğeleri ait derecelendirme puanlarını değiştirmek için kullanılan `DateTimeOffset` alanları. Örneğin, son tarihi olan bir öğe eski öğeleri daha yüksek sıralanabilir.<br /><br /> Mevcut yakın öğelere öğeleri daha yüksek daha fazla gelecekte sıralanabilir şekilde, aynı zamanda gelecek tarihler içeren Takvim etkinlikleri gibi derece öğeleri mümkün olduğunu unutmayın.<br /><br /> Geçerli Hizmet sürümü, geçerli saate bir aralığın sonuna düzeltilecektir. Diğer ucuna göre daha önce bir zamandır `boostingDuration`. Negatif bir kez gelecekte bir dizi artırmak üzere kullanmak `boostingDuration`.<br /><br /> Puanlama profili için en fazla değişiklikleri artırma ve en küçük aralık ilişkilendirme ile belirlenen oranı uygulanır (aşağıdaki şekilde bakın). Uygulanan artırırken faktörü tersine çevirmek için 1'den küçük boost faktörü seçin.|  
|`freshness` &#124; `boostingDuration`|Belirli bir belge için artırma işleminin duracağı sona erme dönemini belirler. Bkz: [ayarlamak boostingDuration](#bkmk_boostdur) söz dizimi ve örnekler için aşağıdaki bölümde yer.|  
|`distance`|Puanlama işlevi etkilemek için kullanılan uzaklık bağlı belgeleri puanı kapatın veya bir başvuru coğrafi konumu göreli oldukları kadar. Başvuru konum bir parametre sorguda bir parçası olarak verilir (kullanarak `scoringParameterquery` dize seçeneği) bir lon lat bağımsız değişken olarak.|  
|`distance` &#124; `referencePointParameter`|Sorgularda başvuru konumu olarak kullanılacak iletilecek parametre. `scoringParameter` bir sorgu parametresidir. Bkz: [arama belgeleri &#40;Azure arama hizmeti REST API'si&#41; ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) Sorgu parametrelerinin açıklamaları.|  
|`distance` &#124; `boostingDistance`|Burada artırma aralığının bittiği başvuru konumundan olan mesafeyi kilometre cinsinden gösteren bir sayı.|  
|`tag`|Puanlama işlevi etiket etiketleri belgelerde ve arama sorgularını temel belgeleri puanı etkilemek için kullanılır. Arama sorgusu ortak bir etikete sahip belgeler artırdı. Her arama isteği Puanlama parametre olarak sağlanan arama sorgusu için etiketleri (kullanarak `scoringParameterquery` dize seçeneği).|  
|`tag` &#124; `tagsParameter`|Belirli bir istek için etiket belirtmek üzere sorgularda iletilecek parametre. `scoringParameter` bir sorgu parametresidir. Bkz: [arama belgeleri &#40;Azure arama hizmeti REST API'si&#41; ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) Sorgu parametrelerinin açıklamaları.|  
|`functionAggregation`|İsteğe bağlı. Yalnızca ne zaman işlevleri belirtilir geçerlidir. Geçerli değerler şunlardır: sum (varsayılan), ortalama, minimum, maksimum ve firstMatching. Bir arama puanı birden çok işlevleri dahil olmak üzere birden çok değişkenlerinden hesaplanan tek değerdir. Bu öznitelik nasıl artırıyor tüm işlevleri tek bir toplamada birleştirilir, artırma gösterir, ardından temel belge puanına uygulanır. Temel puan dayanır [tf-IDF](http://www.tfidf.com/) hesaplanan değer belge ve arama sorgusu.|  
|`defaultScoringProfile`|Bir arama talebi yoksa Puanlama profili belirtilmediyse, ardından varsayılan Puanlama kullanılır yürütülürken ([tf-IDF](http://www.tfidf.com/) yalnızca).<br /><br /> Puanlama profili adı varsayılan arama isteğinde belirli bir profil verildiğinde, bu profili kullanmak Azure Search neden burada ayarlayabilirsiniz.|  

##  <a name="bkmk_interpolation"></a> Kümesi ilişkilendirme  
 İlişkilendirme Puanlama için kullanılan eğimi şeklini ayarlamanıza olanak sağlar. Puanlama düşük olarak yüksek olduğundan, eğim her zaman azalan ancak ilişkilendirme aşağı eğimi eğrisini belirler. Aşağıdaki ilişkilendirmeleri kullanılabilir:  

|||  
|-|-|  
|`Linear`|Maksimum ve minimum aralığında olan öğeleri için öğeye uygulanan boost sürekli olarak azalan bir süre içinde yapılır. Puanlama profili varsayılan ilişkilendirme doğrusal değildir.|  
|`Constant`|Başlangıç ve bitiş aralığı içinde olan öğeleri sıralama sonuçları sürekli artırma uygulanır.|  
|`Quadratic`|Sürekli olarak azalan boost olan bir doğrusal enterpolasyon kolaylığına dereceden başlangıçta daha küçük bir hızda azaltır ve bitiş aralığı yaklaştığında, bir çok daha yüksek aralığında azaltır. Bu ilişkilendirme seçeneği Puanlama işlevleri etiketine izin verilmez.|  
|`Logarithmic`|Sürekli olarak azalan boost olan bir doğrusal enterpolasyon kolaylığına Logaritmik başlangıçta daha yüksek bir hızda azaltır ve bitiş aralığı yaklaştığında, bir çok daha küçük aralığında azaltır. Bu ilişkilendirme seçeneği Puanlama işlevleri etiketine izin verilmez.|  

 ![Grafikte sabit, doğrusal, ikinci derece, log10 satırları](media/scoring-profiles/azuresearch_scorefunctioninterpolationgrapht.png "AzureSearch_ScoreFunctionInterpolationGrapht")  

##  <a name="bkmk_boostdur"></a> Kümesi boostingDuration  
 `boostingDuration` bir özniteliğidir `freshness` işlevi. Bir süre sonu ayarlamak için kullandığınız süre sonra belirli bir belge için artırma hangi durdurur. Örneğin, 10 günlük bir promosyon dönemi için bir ürün veya marka artırmak için bu belgeleri "P10D" olarak 10 gün süreyle belirtmeniz gerekir.  

 `boostingDuration` bir XSD "dayTimeDuration" değeri (ISO 8601 süre değerinin sınırlı alt kümesi) biçimlendirilmiş olması gerekir. Bu modelidir: "P [nD] [T [nH] [nM] [nS]]".  

 Aşağıdaki tabloda, çeşitli örnekleri sağlar.  

|Süre|boostingDuration|  
|--------------|----------------------|  
|1 gün|"P1D"|  
|2 gün 12 saat|"P2DT12H"|  
|15 dakika|"PT15M"|  
|30 gün, 5 saat, 10 dakika, ve 6.334 saniye|"P30DT5H10M6.334S"|  

 Daha fazla örnek için bkz. [XML şeması: Veri türleri (W3.org web sitesi)](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration).  

## <a name="see-also"></a>Ayrıca bkz.  
 [Azure Search Service REST](https://docs.microsoft.com/rest/api/searchservice/)   
 [Dizin oluşturma &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)   
 [Azure Search .NET SDK'sı](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
