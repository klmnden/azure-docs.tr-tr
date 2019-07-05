---
title: Yerelleştirme desteğini Azure haritalar | Microsoft Docs
description: Azure haritalar Hizmetleri için desteklenen diller hakkında bilgi edinin
author: walsehgal
ms.author: v-musehg
ms.date: 06/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: a9446301cc4bb46c989223ad020c7a8e8b353ad3
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446180"
---
# <a name="localization-support-in-azure-maps"></a>Azure haritalar yerelleştirme desteği

Azure haritalar, çeşitli diller ve ülke/bölgeye göre görünümleri destekler. Bu makalede, desteklenen diller ve Azure haritalar uygulamanız rehberlik edecek görünümler sağlar.


## <a name="azure-maps-supported-languages"></a>Azure haritalar desteklenen diller

Azure haritalar Hizmetleri genelinde çeşitli dillerde yerelleştirilmiş. Aşağıdaki tabloda, her hizmet için desteklenen dil kodlarını sağlar.  
  

| id         | Ad                   |  Haritalar | Ara | Yönlendirme | Trafik olayları | JS harita denetimi | Saat dilimi |
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

> [!Note]
> Azure haritalar aşağıdaki ülkelerde/bölgelerde 1 Ağustos 2019 üzerinde kullanıma sunacağımız:
>  * Arjantin
>  * Hindistan
>  * Fas
>  * Pakistan
>
> 1 Ağustos 2019 sonra **görünümü** parametre ayarı, yukarıda listelenen yeni bölgeler/ülkeler için döndürülen harita içeriğinin tanımlamak. Hizmetlerinizi kullanmaya SDK'ları ve REST API'leri için gerektiği gibi görünümü parametresini ayarladığınızdan emin olmak için önerilir.
>  
>
>  **REST API:**
>  
>  Görünüm parametresini gerekli olarak ayarladığınızdan emin olun. Azure haritalar Hizmetleri aracılığıyla hangi ekleyemeyebilir tartışmalı İçerik kümesi döndürülür görünümü parametre belirtir. 
>
>  Etkilenen Azure haritalar REST Hizmetleri:
>    
>    * Harita kutucuğunu Al
>    * Harita resminin Al 
>    * Belirsiz arama Al
>    * Arama POI Al
>    * Arama POI kategorisine sahip olursunuz
>    * Arama yakındaki Al
>    * Arama adresini alma
>    * Yapılandırılmış arama adresi alın
>    * Arama adresini geri alma
>    * Arama ters arası sokak adresi alın
>    * Geometri içinde POST arama
>    * POST arama adresi Batch önizlemesi
>    * POST arama adresi ters Batch önizlemesi
>    * Yol boyunca POST arama
>    * POST arama belirsiz Batch önizlemesi
>
>    
>  **SDK'ları:**
>
>  Görünüm parametresi gerekli olarak ayarladıysanız ve Web SDK'sı ve Android SDK'ın en son sürümüne sahip olun. Etkilenen SDK'ları:
>
>    * Azure haritalar Web SDK'sı
>    * Azure haritalar Android SDK'sı


Azure haritalar **görünümü** parametresi ("kullanıcı bölge parametre olarak" da denir), ISO-3166 ülke kodu'de içeriği hangi ekleyemeyebilir kümesi belirterek bu ülke/bölge tartışmalı için doğru eşlemeleri gösterir iki harf Kenarlıklar ve Haritada görüntülenen etiketler de dahil olmak üzere Azure haritalar Hizmetleri aracılığıyla döndürdü. 

Varsayılan görünüm parametre kümesine **birleşik**istekte tanımladıysanız bile. Bu, kullanıcılarınızın konumunu belirlemek ve ardından görünümü bu konum için doğru parametre sizin sorumluluğunuzdur. Alternatif olarak, ayarlanacak seçeneğiniz ' görünüm = Auto', istek IP adresine göre harita verileri döndürülür.  Azure haritalar görünümü parametresinde, ilgili yasalara uyacağınızı belirtir kullanılmalıdır, olanlar dahil olmak üzere burada haritalar, resimler ve olduğunuz diğer verileri ve üçüncü taraf içeriğini Azure haritalar erişim yetkisi ülkenin ilgili eşleme kullanılabilir hale getirilir.


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
| Otomatik         | İsteğin IP adresine göre harita verilerini döndürür.|   ✓   |    ✓   |     ✓          |
| Birleşik      | Birleşik görünüm (diğer)                  |   ✓   |   ✓     |     ✓          |
