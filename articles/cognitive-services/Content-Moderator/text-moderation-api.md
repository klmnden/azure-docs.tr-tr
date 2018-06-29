---
title: Azure içerik aracı - metin denetleme | Microsoft Docs
description: Metin denetleme metin, PII, olası istenmeyen ve özel koşulları listeler için kullanın.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/30/2018
ms.author: sajagtap
ms.openlocfilehash: 6924807a64cec074d9688eaad158bb9bb638f6bb
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085768"
---
# <a name="text-moderation"></a>Metin denetimi

İçerik denetleyicinin makine destekli metin yönetimini kullanın ve [İnsan gözden geçirme](Review-Tool-User-Guide/human-in-the-loop.md) metin içeriği Orta özellikleri.

Engelleme, onaylayabilir ya da, ilkeleri ve eşikleri bağlı içeriğini gözden geçirin. İnsan yönetimini ortamlarının genişletmek için burada iş ortakları, çalışanlar ve tüketicilerin metin içeriği oluşturmak kullanın. Bunlar, sohbet odaları, tartışma panoları, chatbots, e-ticaret kataloglar ve belgeleri içerir. 

Hizmet yanıtı aşağıdaki bilgileri içerir:

- Uygunsuz metin: çeşitli dillerde saygısız içerikli terimlerin yerleşik listesiyle terim tabanlı eşleştirme
- Sınıflandırma: Makine destekli sınıflandırma üç kategoride
- Kişisel bilgiler (PII)
- Otomatik olarak düzeltileceğini metin
- Özgün metin
- Dil

## <a name="profanity"></a>Uygunsuz metin

API tüm saygısız içerikli koşullarını herhangi birini algılarsa, [desteklenen diller](Text-Moderation-API-Languages.md), söz konusu hükümleri yanıta dahil edilir. Yanıt ayrıca konumlarını içerir (`Index`) özgün metin. `ListId` JSON başvuruyor bulunan Koşulları'nı aşağıdaki örnekteki [özel terim listeleri](try-terms-list-api.md) varsa.

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 0,
        "Term": "crap"
    }

> [!NOTE]
> İçin **dil** parametresi, Ata `eng` veya makine destekli görmek için boş bırakın **sınıflandırma** yanıt (Önizleme özelliği). **Bu özellik yalnızca İngilizce destekler**.
>
> İçin **uygunsuz metin koşulları** algılama, kullanım [ISO 639-3 kodu](http://www-01.sil.org/iso639-3/codes.asp) , bu konuda listelenen desteklenen diller makale veya bu alanı boş bırakın.

## <a name="classification"></a>Sınıflandırma

İçerik denetleyici makine destekli **metin sınıflandırma özelliği** destekleyen **yalnızca İngilizce**, ve olası istenmeyen içerik algılamaya yardımcı olur. Bayrak eklenmiş içerik, içerik bağlı olarak uygun olarak uygunluk. Her kategori olasılığını iletir ve İnsan İnceleme önerebilir. Özellik bir modeli olası kullanım, negatif veya discriminatory dil tanımlamak için kullanır. Bu argo, kısaltılmış sözcükler, gözden geçirme için rahatsız edici ve bilerek yanlış yazılmış sözcükleri içerir. 

JSON Ayıkla içinde aşağıdaki Ayıkla bir örnek çıkış şunları gösterir:

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
    }

### <a name="explanation"></a>Açıklama

- `Category1` olası durum cinsel açık veya bazı durumlarda yetişkinlere yönelik olarak kabul dilinin ifade eder.
- `Category2` olası durum cinsel müstehcen veya bazı durumlarda yetişkin olarak kabul dilinin ifade eder.
- `Category3` Belirli durumlarda rahatsız edici olabilecek dil olası varlığını ifade eder.
- `Score` 0 ile 1 arasında değil. Yüksek puanı kategori uygulanabilen yüksek modeli tahmin etmektir. Bu önizleme el ile kodlanmış sonuçlar yerine bir istatistik modeli kullanır. Her kategoride gereksinimlerinizi için nasıl hizalandığını belirlemek için kendi içerikle sınama öneririz.
- `ReviewRecommended` true veya false iç puan üzerinde eşikleri bağlı değil. Müşteriler bu değeri kullanın ya da kendi içerik ilkelerine dayalı özel eşikler karar değerlendirmelisiniz.

## <a name="personally-identifiable-information-pii"></a>Kişisel bilgiler (PII)

PII özelliği bu bilgileri olası varlığını algılar:

- E-posta adresi
- ABD posta adresi
- IP adresi
- ABD telefon numarası
- Birleşik Krallık telefon numarası
- Sosyal Güvenlik numarası (SSN)

Aşağıdaki örnek, bir örnek yanıt gösterir:

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
            "Index": 212
            }, {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 208
            }, {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 228
            }, {
            "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 245
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
            "Index": 267
            }]
        }

## <a name="auto-correction"></a>Otomatik Düzeltme

Giriş metni olduğunu varsayın ('lzay' ve 'f0x' kasıtlı):

    The qu!ck brown f0x jumps over the lzay dog.

Otomatik düzeltme için isteyin, yanıt metni düzeltilmiş sürümünü içerir:

    The quick brown fox jumps over the lazy dog.

## <a name="creating-and-managing-your-custom-lists-of-terms"></a>Terimler, özel listeleri oluşturma ve yönetme

Varsayılan olarak genel koşulları listesini çoğu zaman harika çalışırken, iş gereksinimleriniz için belirli terimleri karşı ekran isteyebilirsiniz. Örneğin, kullanıcılar tarafından gönderileri rekabet tüm marka adlarından filtrelemenize isteyebilirsiniz.

> [!NOTE]
> Maksimum sınırı yoktur **5 terim listeler** her listesine ile **10.000 koşulları aşmaması**.
>

Aşağıdaki örnek, eşleşen liste kimliği gösterir:

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 231.
        "Term": "crap"
    }

İçerik denetleyici sağlayan bir [terim listesi API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) özel terim yönetmek için işlemleri listeler. İle başlayan [terim listeler API konsol](try-terms-list-api.md) ve REST API kod örnekleri kullanır. Ayrıca kullanıma [terim listeler .NET quickstart](term-lists-quickstart-dotnet.md) Visual Studio ve C# ile hakkında bilginiz varsa.

## <a name="next-steps"></a>Sonraki adımlar

Test sürücü [metin denetleme API konsol](try-text-api.md) ve REST API kod örnekleri kullanır. Ayrıca kullanıma [metin denetleme .NET quickstart](text-moderation-quickstart-dotnet.md) Visual Studio ve C# ile bilginiz varsa.
