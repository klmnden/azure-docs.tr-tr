---
title: Skillset oluşturun (REST API sürümü 2017-11-11-Önizleme =)-Azure arama | Microsoft Docs
description: Bir skillset iyileştirmesini ardışık düzen oluşturan bilişsel becerileri koleksiyonudur.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: language-reference
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: a5277d4137ede5fe6dacf993413eefd7c9e46ad4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-skillset-api-version2017-11-11-preview"></a>Skillset oluşturun (API sürümü 2017-11-11-Önizleme =)

**Uygulandığı öğe:** api-version-2017-11-11-Preview

Bir skillset doğal dil işleme ve diğer dönüştürmeleri için kullanılan bilişsel becerileri koleksiyonudur. Adlandırılmış varlık ayıklama, diğerlerinin yanı sıra mantıksal sayfalar halinde metin Öbekleme anahtar tümcecik ayıklama yetenekleri içerir.

Skillset kullanmak için bir Azure Search dizin oluşturucu başvuru ve veri içeri aktarma, dönüşümler ve iyileştirmesini çağırma ve bir dizine çıkış alanlarını eşleme için dizin oluşturucu çalıştırın. Bir skillset, üst düzey kaynak olmakla birlikte yalnızca dizin oluşturucu işleme içinde çalışır durumda. Üst düzey bir kaynak olarak bir skillset kez tasarlayın ve birden çok dizin oluşturucular başvuru. 

Azure Search'te bir HTTP PUT veya POST isteği aracılığıyla bir skillset ifade. PUT için istek gövdesini hangi becerileri çağrılan belirten bir JSON Şeması ' dir. Yetenekler bir dönüşüm çıktısını başka bir yere giriş olur giriş-çıkış ilişkilendirmeleri aracılığıyla birlikte zincirlenir.

Bir skillset en az bir yetenek olması gerekir. Yetenekler maksimum sayısına kuramsal bir sınır yoktur, ancak üç beş ortak bir yapılandırmadır.  


> [!NOTE]
> Bilişsel Arama, genel önizlemededir ve beceri yürütmesi şu anda ücretsiz olarak sunulur. İlerleyen zamanlarda bu özelliğin fiyatlandırması duyurulacaktır.

```http  
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```  

## <a name="request"></a>İstek  
 HTTPS tüm hizmet istekleri için gereklidir. **Oluşturma Skillset** isteği skillset adlı URL'SİNİN bir parçası olarak bir PUT yöntemini kullanan olacak oluşturulan. Skillset yoksa, oluşturulur. Zaten varsa, yeni tanımına güncelleştirilir. Aynı anda yalnızca bir skillset KOYABİLİRSİNİZ dikkat edin.  

 Skillset adı aşağıdaki gereksinimleri karşılamalıdır:

- Küçük harf olması
- Başlamalı ve bir harf veya sayı ile bitmelidir
- Eğik çizgi veya nokta olmadığından sahip
- 128 karakterden daha azını sahip 

Tireler ardışık olmayan sürece skillset adı bir harf veya sayı ile başlattıktan sonra kalan adının herhangi harf, sayı ve tire içerebilir.  

**API sürümü** parametresi gereklidir. Yalnızca sürüm `2017-11-11-Preview`. Bkz: [API sürümleri Azure Search'te](https://go.microsoft.com/fwlink/?linkid=834796) tüm sürümlerin listesi için. 


### <a name="request-headers"></a>İstek üst bilgileri  

 Aşağıdaki tabloda gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.  

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. Bu ayar `application/json`|  
|*api anahtarı:*|Gereklidir. `api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmetinize benzersiz bir dize değeri değil. **Oluşturma Skillset** isteği içermelidir bir `api-key` üstbilgi yönetici anahtarınızı (aksine, bir sorgu anahtarı) ayarlayın.|  

İstek URL'si oluşturmak için hizmet adı da gerekir. Her iki hizmet adını alabilir ve `api-key` Azure portalında, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.  

### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi  

Daha fazla isteğe bağlı ad ve açıklama parametreleri yanı sıra yetenekler tam olarak belirtilen veya istek gövdesi birini oluşan skillset tanımı içerir.  

İstek yükünde yapılandırılması söz dizimi aşağıdaki gibidir. Bu makalenin sonraki bölümlerinde ve ayrıca bir örnek istek sağlanan [bir skillset tanımlamak nasıl](cognitive-search-defining-skillset.md).  

```
{   
    "name" : "Required for POST, optional for PUT. Friendly name of the skillset",  
    "description" : "Optional. Anything you want, or null",  
    "Skills" : "Required. An array of skills. Each skill has an odata.type, name, input and output parameters",  
}  
```

### <a name="request-example"></a>Örnek istek
 Aşağıdaki örnek, Finans belgeleri topluluğu zenginleştirmek için kullanılan bir skillset oluşturur.

```http
PUT https://[servicename].search.windows.net/skillsets/financedocenricher?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

İstek gövdesi bir JSON belgedir. Zaman uyumsuz olarak, bağımsız olarak, bir madde işleme iki becerileri bu belirli skillset kullanan `contents` iki farklı Dönüşümlerin olarak. Alternatif olarak, başka bir giriş olarak bir dönüşümün çıktısını yönlendirebilirsiniz. Daha fazla bilgi için bkz: [bir skillset tanımlamak nasıl](cognitive-search-defining-skillset.md).

```json
{
  "name": "financedocenricher",
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
  ]
}
```

## <a name="response"></a>Yanıt  

 Başarılı bir istek için "201 oluşturuldu" durum kodu görmeniz gerekir.  

 Varsayılan olarak, yanıt gövdesi oluşturulduğu skillset tanımı için JSON içerir. Ancak, döndürülecek Prefer istek üstbilgisi ayarlarsanız = en az, yanıt gövdesi boş olur ve başarı durumunu kodu olacaktır "204 İçerik yok" "201 oluşturuldu" yerine. Bu, PUT veya POST skillset oluşturmak için kullanılıp kullanılmamasına bakılmaksızın geçerlidir.   

## <a name="see-also"></a>Ayrıca bkz.

+ [Bilişsel arama genel bakış](cognitive-search-concept-intro.md)
+ [Hızlı Başlangıç: Try bilişsel arama](cognitive-search-quickstart-blob.md)
+ [Öğretici: Azure BLOB'ları dizin oluşturma ile zenginleştirilmiş](cognitive-search-tutorial-blob.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
+ [Alanları eşleme](cognitive-search-output-field-mapping.md)
+ [Özel bir arabirim tanımlama](cognitive-search-custom-skill-interface.md)
+ [Örnek: özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)
+ [Önceden tanımlanmış sklls](cognitive-search-predefined-skills.md)