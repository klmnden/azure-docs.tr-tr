---
title: Metin denetimi - Content Moderator
description: Metin denetimi, metin, PII, olası istenmeyen ve koşulları özel listeler için kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 5a1007f2408b48c96f5eeaf585b94c8caa7ceb45
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757753"
---
# <a name="learn-text-moderation-concepts"></a>Metin denetimi kavramları öğrenin

Content Moderator'ın makine destekli metin denetimi kullanın ve [insan tarafından İnceleme](Review-Tool-User-Guide/human-in-the-loop.md) metin içeriğini denetlemek özellikleri.

Engelleme, onaylayabilir ya da, ilkeleri ve eşiklere göre içeriği gözden geçirin. İnsan tarafından denetim ortamların genişletmek için iş ortakları, çalışanlar ve tüketiciler metin içeriği burada oluşturun kullanın. Bunlar, sohbet odaları, tartışma panosu, sohbet robotları, e-ticaret katalogları ve belgelerini içerir. 

Hizmet yanıt aşağıdaki bilgileri içerir:

- Küfür: çeşitli dillerde Küfürlü Koşulları'nın yerleşik listesiyle terimi tabanlı eşleşen
- Sınıflandırma: üç kategoride makine destekli sınıflandırma
- Kişisel veriler
- Otomatik olarak düzeltti metin
- Orijinal metni
- Dil

## <a name="profanity"></a>Küfür

API birinde Küfürlü tüm koşullar algılarsa [desteklenen diller](Text-Moderation-API-Languages.md), bu koşullarında yanıta dahil edilir. Yanıt konumlarını da içeren (`Index`) özgün metin. `ListId` Aşağıdaki örnekte bulunan koşulları başvurduğu JSON [özel terim listeleri](try-terms-list-api.md) varsa.

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
> İçin **küfür koşulları** algılama, kullanımı [ISO 639-3 kodu](http://www-01.sil.org/iso639-3/codes.asp) bu konuda listelenen desteklenen dillerin makale veya boş bırakın.

## <a name="classification"></a>Sınıflandırma

Content Moderator makine destekli **metin sınıflandırma özelliği** destekler **yalnızca İngilizce**, ve potansiyel olarak istenmeyen içeriği algılamaya yardımcı olur. Bağlama bağlı olarak uygun olarak işaretlenen içeriğin denetlenmesini. Her kategori olasılığını iletmez ve insan tarafından İnceleme önerebilir. Özelliği, olası uygunsuz, negatif veya discriminatory dil tanımlamak için eğitilen bir modeli kullanır. Bu argo kullanımlar, kısaltılmış sözcükler, gözden geçirme için rahatsız edici ve bilerek yanlış yazılan sözcükleri içerir. 

Aşağıdaki Ayıkla JSON extract içindeki bir örnek çıktı gösterilmektedir:

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

- `Category1` olası durum cinsel açık veya bazı durumlarda yetişkinlere yönelik olarak kabul edilebilir dilinin ifade eder.
- `Category2` olası durum cinsel müstehcen veya bazı durumlarda olgun düşünülebilecek dilinin ifade eder.
- `Category3` Belirli durumlarda rahatsız edici olduğu düşünülebilecek dil olası varlığını gösterir.
- `Score` 0 ile 1 arasındadır. Yüksek puanı, kategori uygun olabilir yüksek modeli tahmin etmektir. Bu özellik, el ile kodlanmış sonuçları yerine istatistiksel bir model kullanır. Her kategori için gereksinimlerinizi nasıl hizalandığını belirlemek için kendi içeriğe sahip test etmenizi öneririz.
- `ReviewRecommended` true veya false iç puanına göre eşikleri bağlı değil. Müşteriler, bu değeri kullanın veya kendi içerik ilkelere dayalı özel eşikler karar değerlendirmelisiniz.

## <a name="personal-data"></a>Kişisel veriler

PII özelliği, bu bilgilerin olası varolup olmadığını algılar:

- E-posta adresi
- ABD posta adresi
- IP adresi
- ABD telefon numarası
- UK telefon numarası
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

Giriş metni olduğunu varsayın ('lzay' ve 'f0x' bilinçli yapıldığı):

    The qu!ck brown f0x jumps over the lzay dog.

Otomatik düzeltmeye sorarsanız yanıt düzeltilmiş metin sürümünü içerir:

    The quick brown fox jumps over the lazy dog.

## <a name="creating-and-managing-your-custom-lists-of-terms"></a>Koşulları, özel listeleri oluşturma ve yönetme

Varsayılan olarak genel koşulları listesini çoğu durum için mükemmel çalışırken, iş gereksinimlerinize özel terimleri karşı ekran isteyebilirsiniz. Örneğin, kullanıcılar tarafından gönderileri herhangi rekabetçi marka adları filtrelemek isteyebilirsiniz.

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır**.
>

Aşağıdaki örnek, eşleşen liste kimliği gösterir:

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 231.
        "Term": "crap"
    }

Content Moderator sağlayan bir [terim listesi API'si](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) özel terim yönetme işlemleri listeler. İle başlayan [terim listeleri API Konsolu](try-terms-list-api.md) ve REST API kod örnekleri kullanın. Ayrıca kullanıma [terim listeleri .NET Hızlı Başlangıç](term-lists-quickstart-dotnet.md) Visual Studio ve C# ile deneyiminiz varsa.

## <a name="next-steps"></a>Sonraki adımlar

Sürücü test [metin denetimi API'si konsol](try-text-api.md) ve REST API kod örnekleri kullanın. Ayrıca kullanıma [metin denetimi .NET hızlı](text-moderation-quickstart-dotnet.md) Visual Studio ve C# ile ilgili bilgi sahibi değilseniz.
