---
title: "Azure uygulama ağ geçidi için SSL ilkesine genel bakış | Microsoft Docs"
description: "Azure uygulama ağ geçidi, SSL ilkesini yapılandırmak nasıl sağlar hakkında bilgi edinin"
services: application gateway
documentationcenter: na
author: amsriva
manager: 
editor: 
tags: azure resource manager
ms.service: application gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure services
ms.date: 08/03/2017
ms.author: amsriva
ms.openlocfilehash: ec36af282bbfdc22ff88082412dd18cd2a85f245
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="application-gateway-ssl-policy-overview"></a>Uygulama ağ geçidi SSL ilkesine genel bakış

SSL sertifika yönetimi merkezileştirmek ve arka uç sunucu grubundan şifreleme ve şifre çözme yükünü azaltmak için Azure uygulama ağ geçidi'ni kullanabilirsiniz. Ayrıca işleme bu merkezi SSL, kuruluş güvenlik gereksinimlerinize uygun bir merkezi SSL ilke belirtmenize olanak sağlar. Bu güvenlik yönergeleri yanı sıra uyumluluk gereksinimlerini karşılamanıza yardımcı olur ve önerilen uygulamalar.

SSL ilke denetimi SSL Protokolü sürüm hem de şifre paketleri ve şifre bir SSL el sıkışması sırasında kullanılan order içerir. Uygulama ağ geçidi SSL İlkesi denetlemek için kullanılan iki mekanizma sunar. Önceden tanımlanmış bir ilke veya özel bir ilke kullanabilirsiniz.

## <a name="predefined-ssl-policy"></a>Önceden tanımlanmış SSL İlkesi

Uygulama ağ geçidi üç önceden tanımlanmış güvenlik ilkeleri vardır. Ağ geçidi herhangi bir uygun düzeyde bir güvenlik almak için bu ilkeleri ile yapılandırabilirsiniz. Yapılandırılmış olan ay ve yıl ilke adları açıklama eklendi. Her ilke teklifleri farklı SSL protokol sürümleri ve şifre paketleri. En iyi SSL güvenliği sağlamak için en yeni SSL ilkelerle kullanmanızı öneririz.

### <a name="appgwsslpolicy20150501"></a>AppGwSslPolicy20150501

|Özellik  |Değer  |
|---|---|
|Ad     | AppGwSslPolicy20150501        |
|MinProtocolVersion     | TLSv1_0        |
|Varsayılan| TRUE (önceden tanımlanmış hiçbir ilke belirtilen) |
|Şifreleme paketleri     |TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_DHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_DHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_3DES_EDE_CBC_SHA<br>TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA |
  
  ### <a name="appgwsslpolicy20170401"></a>AppGwSslPolicy20170401
  
|Özellik  |Değer  |
|   ---      |  ---       |
|Ad     | AppGwSslPolicy20170401        |
|MinProtocolVersion     | TLSv1_1        |
|Varsayılan| False |
|Şifreleme paketleri     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA |
  
### <a name="appgwsslpolicy20170401s"></a>AppGwSslPolicy20170401S

|Özellik  |Değer  |
|---|---|
|Ad     | AppGwSslPolicy20170401S        |
|MinProtocolVersion     | TLSv1_2        |
|Varsayılan| False |
|Şifreleme paketleri     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 <br>    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 <br>    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br> |

## <a name="custom-ssl-policy"></a>Özel SSL İlkesi

Önceden tanımlanmış bir SSL İlkesi gereksinimleriniz için yapılandırılmış olması gerekiyorsa, kendi özel SSL İlkesi tanımlamanız gerekir. Özel bir SSL ilke ile desteklenen şifre paketleri ve bunların öncelik sırası yanı sıra desteklemek için en düşük SSL Protokolü sürüm tam denetime sahip olursunuz.
 
### <a name="ssl-protocol-versions"></a>SSL protokol sürümleri

* SSL 2.0 ve 3.0, tüm uygulama ağ geçitleri için varsayılan olarak devre dışıdır. Bu protokol sürümleri yapılandırılabilir değildir.
* Özel bir SSL ilke ağ geçidiniz için en düşük SSL protokolü sürümü olarak aşağıdaki üç protokolden birini seçmek için seçeneği sunar: TLSv1_0, TLSv1_1 ve TLSv1_2.
* Hiçbir SSL ilkesi tanımlanmışsa, tüm üç protokolleri (TLSv1_0, TLSv1_1 ve TLSv1_2) etkinleştirilir.

### <a name="cipher-suites"></a>Şifre paketleri

Uygulama ağ geçidi, özel ilkeniz seçim yapabileceğiniz aşağıdaki şifre paketleri destekler. Şifre paketlerinin sıralama SSL anlaşması sırasında öncelik sırasını belirler.


- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek bir SSL İlkesi yapılandırmak için bkz: istiyorsanız [bir uygulama ağ geçidi yapılandırma SSL ilkesindeki](application-gateway-configure-ssl-policy-powershell.md).
