---
title: Web uygulaması güvenlik duvarı istek boyutu sınırları ve Azure Application Gateway'de - Azure portalı hariç tutma listeleri
description: Bu makale, web uygulaması güvenlik duvarı istek boyutu sınırları hakkında bilgi sağlar ve Azure portal ile Application Gateway yapılandırmasında dışlama listeler.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.workload: infrastructure-services
ms.date: 11/6/2018
ms.author: victorh
ms.openlocfilehash: f89841c7712737d2d55601c6525e975274b4a103
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51036726"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists-public-preview"></a>Web uygulaması güvenlik duvarı istek boyutu sınırları ve hariç tutma listeleri (genel Önizleme)

Azure Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu makalede WAF istek boyutu sınırları ve yapılandırma dışlama listeler.

> [!IMPORTANT]
> WAF istek boyutu sınırları ve hariç tutma listeleri yapılandırması, şu anda genel Önizleme aşamasındadır. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="waf-request-size-limits"></a>WAF istek boyutu sınırları

![İstek boyutu sınırları](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

Web uygulaması güvenlik duvarı, kullanıcıların istek boyutu sınırları içinde alt ve üst sınırları yapılandırmasını sağlar. Aşağıdaki iki boyutu sınırlarını yapılandırma kullanılabilir:

- En fazla istek gövdesi boyutu alanı KB'leri ve denetimler genel istek boyutu sınırı dışında herhangi bir dosya yükler belirtilir. Bu alanın 128 KB'lık maksimum değerine 1 KB'lık minimumdan değişebilir. 128 KB istek gövdesi boyutu için varsayılan değerdir.
- Dosya karşıya yükleme sınırı alanı MB olarak belirtilir ve izin verilen en büyük dosya yükleme boyutunu yönetir. Bu alan 1 MB en düşük değerini ve en fazla 500 MB olabilir. Dosya karşıya yükleme sınırı için varsayılan değer 100 MB'dir.

Ayrıca, WAF istek gövdesi İnceleme açıp kapatmak için yapılandırılabilir bir düğme sunar. İstek gövdesi İnceleme varsayılan olarak etkinleştirilir. İstek gövdesi İnceleme kapalıysa, WAF HTTP ileti gövdesi içeriğini değerlendirmez. Böyle durumlarda, üst bilgiler, tanımlama bilgileri ve URI WAF kurallarını zorunlu tutmak WAF devam eder. İstek gövdesi İnceleme kapalıysa, en büyük istek gövdesi boyutu alan geçerli değildir ve ayarlanamaz. İletiler için istek gövdesi İnceleme kapatmak için WAF gönderilecek 128 KB'tan büyük sağlar, ancak ileti gövdesi güvenlik açıklarına karşı inceledi değil.

## <a name="waf-exclusion-lists"></a>WAF hariç tutma listeleri

![waf exclusion.png](media/application-gateway-waf-configuration/waf-exclusion.png)

WAF hariç tutma listeleri, kullanıcıların belirli bir WAF değerlendirme özniteliklerini atlamak izin verin. Yaygın olarak karşılaşılan örneklerden, Active Directory kimlik doğrulaması veya parola alanı için kullanılan belirteçleri eklenen ' dir. Bir hatalı pozitif sonuç WAF kurallarından tetikleyebilir özel karakterler içermesini gibi öznitelikleri fazladır. Bir öznitelik WAF dışlama listesine eklendikten sonra tüm yapılandırılmış ve etkin bir WAF kural tarafından dikkate almanız değil. Hariç tutma listeleri, genel kapsam içindedir.

Hariç tutma listeleri aşağıdaki öznitelikleri eklenebilir:

* İstek üst bilgileri
* İstek tanımlama bilgileri
* İstek Gövdesi

   * Çok bölümlü form verilerinin
   * XML
   * JSON

Bir tam istek üst bilgisini belirtin, tanımlama bilgisi veya sorgu dizesi özniteliği eşleşme gövde veya kısmi eşleşmeler isteğe bağlı olarak belirtebilirsiniz.

Desteklenen eşleşme ölçütlerini işleçler şunlardır:

- **Eşittir**: Bu işleci için tam bir eşleşme kullanılır. Adlı bir üst bilgi seçmek için örnek olarak **bearerToken**, eşittir işleci olarak seçiciyi kullanın **bearerToken**.
- **İle başlayan**: Belirtilen Seçici değerle başlayan tüm alanları bu işleci eşleşir.
- **İle biten**: Belirtilen seçici değeri ile biten tüm isteği alanları bu işleci eşleşir.
- **İçeren**: Belirtilen seçici değeri içeren tüm isteği alanları bu işleci eşleşir.

Her durumda eşleştirme büyük küçük harfe duyarlı değildir ve normal ifade Seçici izin verilmiyor.

## <a name="next-steps"></a>Sonraki adımlar

WAF ayarlarınızı yapılandırdıktan sonra WAF günlükleri görüntüleme hakkında bilgi edinebilirsiniz. Daha fazla bilgi için [Application Gateway tanılama](application-gateway-diagnostics.md#diagnostic-logging).