---
title: Azure içerik Aracı alanında metin denetleme API'sini kullanarak metin Orta | Microsoft Docs
description: Metin denetleme online konsolunda metin denetleme API'sini kullanarak dediğini.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: ed696c31a886626819414c45eb7995edaf161fff
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352721"
---
# <a name="moderate-text-from-the-api-console"></a>API konsolundan Orta metin

Kullanım [metin denetleme API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) Azure içeriği metin içeriğinizi tarama geçirilmesini içinde. İşlem içeriğiniz uygunsuz metin için tarar ve özel ve paylaşılan blacklists içerikte karşılaştırır.


## <a name="get-your-api-key"></a>API anahtarınızı alın
Çevrimiçi konsolunda API dediğini önce abonelik anahtarınızı gerekir. Bu bulunan **ayarları** sekmesinde **Apim abonelik anahtar Ocp** kutusu. Daha fazla bilgi için bkz: [genel bakış](overview.md).

## <a name="navigate-to-the-api-reference"></a>API referansı gidin
Git [metin denetleme API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f). 

  **Metin - ekran** sayfası açılır.

## <a name="open-the-api-console"></a>API konsolunu açın
İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Metin - ekran sayfa bölge seçimi](images/test-drive-region.png)

  **Metin - ekran** API konsolu açılır.

## <a name="select-the-inputs"></a>Girdi seçin

### <a name="parameters"></a>Parametreler
Metin ekranınızı kullanmak istediğiniz sorgu parametreleri seçin. Bu örnekte, varsayılan değeri kullanın **dil**. İşlemi yürütülmesinin bir parçası olarak otomatik olarak büyük olasılıkla dil algılar çünkü aynı zamanda boş bırakılabilir.

> [!NOTE]
> İçin **dil** parametresi, Ata `eng` veya makine destekli görmek için boş bırakın **sınıflandırma** yanıt (Önizleme özelliği). **Bu özellik yalnızca İngilizce destekler**.
>
> İçin **uygunsuz metin koşulları** algılama, kullanım [ISO 639-3 kodu](http://www-01.sil.org/iso639-3/codes.asp) , bu konuda listelenen desteklenen diller makale veya bu alanı boş bırakın.

İçin **düzeltme**, **PII**, ve **(Önizleme) sınıflandırmak**seçin **doğru**. Bırakın **ListId** alanı boş.

  ![Metin - ekran konsol sorgu parametreleri](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>İçerik türü
İçin **Content-Type**, ekran istediğiniz içerik türünü seçin. Bu örnekte, varsayılan kullanmak **metin/düz** içerik türü. İçinde **Apim abonelik anahtar Ocp** kutusuna, abonelik anahtarınızı girin.

### <a name="sample-text-to-scan"></a>Örnek metin taramak için
İçinde **istek gövdesinde** kutusuna, belirli bir metin girin. Aşağıdaki örnek kasıtlı bir yazım hatası metinde gösterir.

> [!NOTE]
> Aşağıdaki örnek metin geçersiz sosyal güvenlik numarası kasıtlıdır. Amacı iletmek örnek giriş ve çıkış biçimi sağlamaktır.

```
    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.
    Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="text-classification-feature"></a>Metin sınıflandırma özelliği

Aşağıdaki örnekte, içerik denetleyicinin makine destekli metin sınıflandırma yanıt görürsünüz. Bu olası istenmeyen içerikleri algılama yardımcı olur. Bağlam bağlı olarak uygun olarak işaretli içerik olarak kabul. Her kategori olasılığını saymayı ek olarak, içerik İnsan gözden önerebilir. Özellik bir modeli olası kullanım, negatif veya discriminatory dil tanımlamak için kullanır. Bu argo, kısaltılmış sözcükler, gözden geçirme için rahatsız edici ve bilerek yanlış yazılmış sözcükleri içerir. 

#### <a name="explanation"></a>Açıklama

- `Category1` cinsel açık veya bazı durumlarda yetişkinlere yönelik olarak kabul dil olası varlığını temsil eder.
- `Category2` cinsel müstehcen veya bazı durumlarda yetişkin olarak kabul dil olası varlığını temsil eder.
- `Category3` Belirli durumlarda rahatsız edici olabilecek dil olası varlığını temsil eder.
- `Score` 0 ile 1 arasında değil. Yüksek puanı kategori uygulanabilen yüksek modeli tahmin etmektir. Bu önizleme el ile kodlanmış sonuçlar yerine bir istatistik modeli kullanır. Her kategoride gereksinimlerinizi için nasıl hizalandığını belirlemek için kendi içerikle sınama öneririz.
- `ReviewRecommended` true veya false iç puan üzerinde eşikleri bağlı değil. Müşteriler bu değeri kullanın ya da kendi içerik ilkelerine dayalı özel eşikler karar değerlendirmelisiniz.

### <a name="analyze-the-response"></a>Yanıt Çözümle
Aşağıdaki yanıtı çeşitli Öngörüler API'SİNDEN gösterir. Olası uygunsuz metin, PII, sınıflandırma (Önizleme) ve otomatik olarak düzeltileceğini sürümünü içerir.

> [!NOTE]
> Makine destekli 'Sınıflandırma' özellik Önizleme aşamasındadır ve yalnızca İngilizce destekler.

```
{
    "OriginalText": "Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.\r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.\r\nAlso, 544-56-7788 looks like a social security number (SSN).",
    "NormalizedText": "Is this a grabage or crap email abcdef@ abcd. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. \r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. \r\nAlso, 544- 56- 7788 looks like a social security number ( SSN) .",
"Misrepresentation": null,
    "PII": {
            "Email": [{
                "Detected": "abcdef@abcd.com",
                "SubType": "Regular",
                "Text": "abcdef@abcd.com",
                "Index": 32
                }],
            "IPA": [{
                "SubType": "IPV4",
                "Text": "255.255.255.255",
                "Index": 72
                }],
            "Phone": [{
                "CountryCode": "US",
                "Text": "6657789887",
                "Index": 56
                }, {
                "CountryCode": "US",
                "Text": "870 608 4000",
                "Index": 211
                }, {
                "CountryCode": "UK",
                "Text": "+44 870 608 4000",
                "Index": 207
                }, {
                "CountryCode": "UK",
                "Text": "0344 800 2400",
                "Index": 227
                }, {
                "CountryCode": "UK",
                "Text": "0800 820 3300",
                "Index": 244
            }],
         "Address": [{
                 "Text": "1 Microsoft Way, Redmond, WA 98052",
                "Index": 89
            }],
            "SSN": [{
                "Text": "999999999",
                "Index": 56
            }, {
                "Text": "999-99-9999",
                "Index": 266
            }]
        },
    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
        },
        "Category2": {
            "Score": 0.12747249007225037
        },
        "Category3": {
            "Score": 0.98799997568130493
        }
        },
    "Language": "eng",
    "Terms": [{
            "Index": 21,
            "OriginalIndex": 21,
            "ListId": 0,
         "Term": "crap"
        }],
    "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        },
     "TrackingId": "2eaa012f-1604-4e36-a8d7-cc34b14ebcb4"
}
```

JSON yanıt tüm bölümlerde ayrıntılı bir açıklaması için başvurmak [metin denetleme API genel bakış](text-moderation-api.md).

## <a name="next-steps"></a>Sonraki adımlar

REST API kodunuzdaki kullanın veya başlayın [metin denetleme .NET quickstart](text-moderation-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
