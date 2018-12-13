---
title: Yaklaşım bilişsel arama beceri - Azure Search
description: Bir Azure Search zenginleştirme ardışık metinden pozitif ve negatif yaklaşım puanını ayıklayın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 4b8913d64a3df8799ba1d73972121ef331aaac81
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314083"
---
#   <a name="sentiment-cognitive-skill"></a>Yaklaşım bilişsel beceri

**Yaklaşım** beceri yapılandırılmamış metinleri pozitif ve negatif sürekliliği ve her kayıt için değerlendirir, 0 ile 1 arasında bir sayısal puan döndürür. Puanın 1’e yakın olması yaklaşımın olumlu olduğunu, 0’a yakın olması ise olumsuz olduğunu gösterir.

> [!NOTE]
> 21 aralık 2018 tarihinden itibaren Bilişsel hizmetler kaynağı bir Azure Search beceri kümesi ile ilişkilendirmek mümkün olmayacak. Bu beceri yürütmesi için ücretlendirme başlatmak için bize izin verir. Bu tarihte, biz de belge çözme aşamasının bir parçası olarak görüntü ayıklama için başlayacağız. Belgelerden metin ayıklama işlemi ek masraf olmadan sağlanmaya devam edecektir.
>
> Var olan konumunda yerleşik yetenek yürütülmesini ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/) . Görüntü ayıklama fiyatlandırma Önizleme fiyatıyla ücretlendirilirsiniz ve üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400). Bilgi [daha fazla](cognitive-search-attach-cognitive-services.md).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu tarafından ölçülen 5000 karakter arasında olmalıdır `String.Length`. Kullanım, yaklaşım Çözümleyicisi göndermeden önce verileri bölün gerekiyorsa [metin bölme beceri](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre Adı |                      |
|----------------|----------------------|
| defaultLanguageCode | (isteğe bağlı) Dil açıkça belirtmeyin belgelerine uygulamak için dil kodu. <br/> Bkz: [desteklenen dillerin tam listesini](../cognitive-services/text-analytics/text-analytics-supported-languages.md) |

## <a name="skill-inputs"></a>Beceri girişleri 

| Adı girin | Açıklama |
|--------------------|-------------|
| metin | Analiz edilecek metin.|
| languageCode  |  (İsteğe bağlı) Kayıt dili belirten bir dize. Bu parametre belirtilmezse, varsayılan değer "en" dir. <br/>Bkz: [desteklenen dillerin tam listesini](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Beceri çıkışları

| Çıkış adı | Açıklama |
|--------------------|-------------|
| puan | 0 ve 1 arasında bir değer çözümlenmiş metin duyarlılığını temsil eder. Olumsuz 0 değerine sahip, nötr yaklaşım 0,5 yakın olması ve pozitif yaklaşımı 1 değerine sahip.|


##  <a name="sample-definition"></a>Örnek tanımı

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
        }
    ]
}
```

##  <a name="sample-input"></a>Örnek Giriş

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
                "languageCode": "en"
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Örnek çıktı

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Notlar
Boş ise, yaklaşım puanını kayıtları için döndürülmez.

## <a name="error-cases"></a>Hata durumları
Bir dil desteklenmiyorsa, bir hata oluşturulur ve hiçbir yaklaşım puanı döndürülür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)