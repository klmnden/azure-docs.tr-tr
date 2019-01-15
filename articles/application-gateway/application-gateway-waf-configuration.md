---
title: Web uygulaması güvenlik duvarı istek boyutu sınırları ve Azure Application Gateway'de - Azure portalı hariç tutma listeleri
description: Bu makale, web uygulaması güvenlik duvarı istek boyutu sınırları hakkında bilgi sağlar ve Azure portal ile Application Gateway yapılandırmasında dışlama listeler.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.workload: infrastructure-services
ms.date: 11/6/2018
ms.author: victorh
ms.openlocfilehash: 4e57181b62a6d9070c0b2e4de5008e47b62c56bf
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54301908"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists"></a>Web uygulaması güvenlik duvarı istek boyutu sınırları ve hariç tutma listeleri

Azure Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu makalede WAF istek boyutu sınırları ve yapılandırma dışlama listeler.

## <a name="waf-request-size-limits"></a>WAF istek boyutu sınırları

![İstek boyutu sınırları](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

Web uygulaması güvenlik duvarı, kullanıcıların istek boyutu sınırları içinde alt ve üst sınırları yapılandırmasını sağlar. Aşağıdaki iki boyutu sınırlarını yapılandırma kullanılabilir:

- En fazla istek gövdesi boyutu alanı KB'leri ve denetimler genel istek boyutu sınırı dışında herhangi bir dosya yükler belirtilir. Bu alanın 128 KB'lık maksimum değerine 1 KB'lık minimumdan değişebilir. 128 KB istek gövdesi boyutu için varsayılan değerdir.
- Dosya karşıya yükleme sınırı alanı MB olarak belirtilir ve izin verilen en büyük dosya yükleme boyutunu yönetir. Orta SKU en fazla 100 MB'ın olsa Bu alan 1 MB en düşük değerini ve en fazla 500 MB büyük SKU örnekleri için sahip olabilir. Dosya karşıya yükleme sınırı için varsayılan değer 100 MB'dir.

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

- **Eşittir**:  Bu işleç, tam bir eşleşme için kullanılır. Adlı bir üst bilgi seçmek için örnek olarak **bearerToken**, eşittir işleci olarak seçiciyi kullanın **bearerToken**.
- **İle başlayan**: Bu işleç, belirtilen Seçici değerle başlayan tüm alanları eşleşir.
- **İle biten**:  Bu işleç, belirtilen seçici değeri ile biten tüm isteği alanları eşleşir.
- **İçeren**: Bu işleç belirtilen seçici değeri içeren tüm isteği alanları eşleşir.

Her durumda eşleştirme büyük küçük harfe duyarlı değildir ve normal ifade Seçici izin verilmiyor.

## <a name="next-steps"></a>Sonraki adımlar

WAF ayarlarınızı yapılandırdıktan sonra WAF günlükleri görüntüleme hakkında bilgi edinebilirsiniz. Daha fazla bilgi için [Application Gateway tanılama](application-gateway-diagnostics.md#diagnostic-logging).
