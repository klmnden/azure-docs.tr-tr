---
title: Yerelleştirme desteğini Azure haritalar | Microsoft Docs
description: Azure haritalar Hizmetleri için desteklenen diller hakkında bilgi edinin
author: walsehgal
ms.author: v-musehg
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 1928185521419006a487a933e2ecba79894a09d3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64686776"
---
# <a name="localization-support-in-azure-maps"></a>Azure haritalar yerelleştirme desteği

Azure haritalar, çeşitli diller ve ülke/bölgeye göre görünümleri destekler. Bu makalede, desteklenen diller ve Azure haritalar uygulamanız rehberlik edecek görünümler sağlar.


## <a name="azure-maps-supported-languages"></a>Azure haritalar desteklenen diller

Azure haritalar Hizmetleri genelinde çeşitli dillerde yerelleştirilmiş. Aşağıdaki tabloda, her hizmet için desteklenen dil kodlarını sağlar.  
  

| Kimlik         | Ad                   |  Haritalar | Ara | Yönlendirme | Trafik olayları | JS harita denetimi | Saat dilimi |
|------------|------------------------|:-----:|:------:|:-------:|:-----------------:|:--------------:|:---------:|
| ZA AF      | Afrikaner dili              |       |    ✓   |    ✓    |                   |                |     ✓     |
| ar-SA      | Arapça                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| AB-ES      | Bask dili                 |       |    ✓   |         |                   |                |     ✓     |
| BG-BG      | Bulgarca              |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| CA-ES      | Katalanca                |       |    ✓   |         |                   |                |     ✓     |
| zh-HanS    | Çince (Basitleştirilmiş)   |       |  zh-CN |         |                   |                |     ✓     |
| zh-HanT    | seçenekleri yerine  | zh-TW |  zh-TW |  zh-TW  |                   |      Zh-TW     |     ✓     |
| hr-HR      | Hırvatça               |       |    ✓   |         |                   |                |     ✓     |
| cs-CZ      | Çekçe                  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| v-DK      | Danca                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| NL-NL      | Felemenkçe                  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| NL-olabilir      | Flamanca (Belçika)        |       |    ✓   |         |                   |                |     ✓     |
| tr-AU      | İngilizce (Avustralya)    |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| tr NZ      | İngilizce (Yeni Zelanda)  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| en-GB      | İngilizce (İngiltere) |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| en-US      | İngilizce (ABD)          |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| et-EE      | Estonca               |       |    ✓   |         |         ✓         |                |     ✓     |
| FI-FI      | Fince                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| fr-FR      | Fransızca                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| fr-CA      | Fransızca (Kanada)      |       |    ✓   |         |                   |                |     ✓     |
| GL ES      | Galiçya dili               |       |    ✓   |         |                   |                |     ✓     |
| de-DE      | Almanca                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| el-GR      | Yunanca                  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| He IL      | İbranice                 |       |    ✓   |         |         ✓         |                |     ✓     |
| yüksek giriş      | Hintçe                  |       |        |         |                   |                |     ✓     |
| hu-HU      | Macarca              |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| ID      | Endonezya dili             |   ✓   |    ✓    |    ✓    |         ✓         |        ✓       |     ✓     |
| İt-IT      | İtalyanca                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| ja-JP      | Japonca               |       |        |         |                   |                |     ✓     |
| kk-KZ      | Kazakça                 |       |    ✓   |         |                   |                |     ✓     |
| ko-KR      | Korece                 |   ✓   |        |    ✓    |                   |        ✓       |     ✓     |
| es-419     | Latin Amerika İspanyolca |       |    ✓   |         |                   |                |     ✓     |
| lv-LV      | Letonca                |       |    ✓   |         |         ✓         |                |     ✓     |
| lt-LT      | Litvanca             |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| ms-MY      | Malay Dili (Latin)          |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| NB-yok      | Norwegian Bokmål       |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| NGT        | Nötr çığır Truth - tüm bölgelerde kullanılabilir durumdaysa yerel betikler için resmi dilleri |   ✓     |        |         |                   |      ✓          |         |
| NGT Latn   | Nötr çığır Truth - Latin exonyms. Latin betik varsa kullanılacak |   ✓     |        |         |                   |        ✓         |          |
| pl-PL      | Lehçe                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| pt-BR      | Portekizce (Brezilya)    |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| pt-PT      | Portekizce (Portekiz)  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| Ro-RO      | Rumence               |       |    ✓    |         |         ✓         |                |     ✓     |
| ru-RU      | Rusça                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| SR-Cyrl-RS | Sırpça (Kiril)     |       |    Sırpça (Kiril) (sr-RS)   |         |                   |                |     ✓     |
| SR-Latn-RS | Sırpça (Latin)        |       |        |         |                   |                |     ✓     |
| SK-SK      | Slovakça              |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| SL SL      | Slovence              |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| es-ES      | İspanyolca                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| es-MX      | İspanyolca (Meksika)       |   ✓   |        |    ✓    |                   |        ✓       |     ✓     |
| sv -SE     | İsveççe                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| TH TH      | Tay Dili                   |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| tr-TR      | Türkçe                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| uk-UA      | Ukrayna dili               |       |    ✓   |         |                   |                |     ✓     |
| VI VN      | Vietnam dili             |       |    ✓   |         |                   |                |     ✓     |


## <a name="azure-maps-supported-views"></a>Azure haritalar desteklenen görünümleri

("Kullanıcı bölge parametre olarak" da denir) azure haritalar görünüm parametresi hangi ekleyemeyebilir kümesini belirtme, ülke/bölge için doğru eşlemeleri Kenarlıklar tartışmalı gösteren 2 harf 3166 ISO ülke kodu, etiketleri harita üzerinde görüntülenir.  Varsayılan görünüm parametre kümesine **"Birleştirilmiş"** .  Görünüm listede yer almayan ülke/bölge "Birleştirilmiş" görünümü için varsayılan olarak kullanılır. Bu, kullanıcılarınızın konumunu belirlemek ve ardından görünümü bu konum için doğru parametre sizin sorumluluğunuzdur. Azure haritalar görünümü parametresinde, ilgili yasalara uyacağınızı belirtir kullanılmalıdır, olanlar dahil olmak üzere burada haritalar, resimler ve olduğunuz diğer verileri ve üçüncü taraf içeriğini Azure haritalar erişim yetkisi ülkenin ilgili eşleme kullanılabilir hale getirilir.

Aşağıdaki tabloda, desteklenen görünümler sağlar.

| Görünüm         | Açıklama                            |  Haritalar | Ara | JS harita denetimi |
|--------------|----------------------------------------|:-----:|:------:|:--------------:|
| AE           | Birleşik Arap Emirlikleri (Arapça görünümü)    |   ✓   |        |     ✓          |
| AR           | Arjantin (Arjantin görünümü)           |   ✓   |    ✓   |     ✓          |
| BH           | Bahreyn (Arapça görünümü)                 |   ✓   |        |     ✓          |
| IN           | Hindistan (Hindistan görünümü)                    |   ✓   |   ✓     |     ✓          |
| IQ           | Irak (Arapça görünümü)                    |   ✓   |        |     ✓          |
| JO           | Ürdün (Arapça görünümü)                  |   ✓   |        |     ✓          |
| KW           | Kuveyt (Arapça görünümü)                  |   ✓   |        |     ✓          |
| LB           | Lübnan (Arapça görünümü)                 |   ✓   |        |     ✓          |
| MA           | Fas (Fas görünümü)                |   ✓   |   ✓     |     ✓          |
| OM           | Umman (Arapça görünümü)                    |   ✓   |        |     ✓          |
| PK           | Pakistan (Pakistan görünümü)              |   ✓   |    ✓    |     ✓          |
| PS           | Filistin Yönetimi (Arapça görünümü)    |   ✓   |        |     ✓          |
| QA           | Katar (Arapça görünümü)                   |   ✓   |        |     ✓          |
| SA           | Suudi Arabistan (Arapça görünümü)            |   ✓   |        |     ✓          |
| SY           | Suriye (Arapça görünümü)                   |   ✓   |        |     ✓          |
| YE           | Yemen (Arapça görünümü)                   |   ✓   |        |     ✓          |
| Birleşik      | Birleşik görünüm (diğer)                  |   ✓   |   ✓     |     ✓          |
