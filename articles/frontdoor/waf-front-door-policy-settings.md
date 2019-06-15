---
title: Web uygulaması güvenlik duvarı ile Azure ön kapı için ilke ayarları
description: Web uygulaması Güvenlik Duvarı (WAF) hakkında bilgi edinin.
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: tyao;kumud
ms.openlocfilehash: 4c2f070e9b3c972f063008df8880b196ddb069cc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61459377"
---
# <a name="policy-settings-for-web-application-firewall-with-azure-front-door"></a>Web uygulaması güvenlik duvarı ile Azure ön kapı için ilke ayarları

Bir Web uygulaması Güvenlik Duvarı (WAF) ilkesi tarafından özel ve yönetilen kurallar kümesi, web uygulamalarına erişim denetlemenizi sağlar. WAF ilke adı benzersiz olmalıdır. Varolan bir adı kullanmaya çalışırsanız, bir doğrulama hatası alırsınız. Bu makalede anlatıldığı gibi yönelik olan ilkeye belirtilen tüm kuralları uygulamak birden çok ilke düzeyi ayarları vardır.

## <a name="waf-state"></a>WAF durumu

WAF ilke ön kapı için aşağıdaki iki durumdan birinde olabilir:
- **Etkin:** Bir ilke etkinleştirildiğinde, WAF gelen istekleri etkin bir şekilde inceliyor ve kuralı tanımlarına göre ilgili eylemleri gerçekleştirir
- **Devre dışı:** - ilke devre dışı bırakıldığında, WAF İnceleme duraklatıldı. Gelen istekler, WAF atlayacaktır ve arka uçları ön kapısı üretim akışında göre gönderilir.

## <a name="waf-mode"></a>WAF modu

WAF İlkesi, aşağıdaki iki modda çalışacak şekilde yapılandırılabilir:

- **Algılama modu** algılama modunda çalıştırdığınızda, WAF değil İzleyici dışındaki herhangi bir eylem gerçekleştirmenizi ve istek ve kendi eşleşen WAF kural WAF günlüklerine Kaydet. Ön kapısı günlük tanılamayı Aç (portal kullanırken bu giderek ulaşılabilecek **tanılama** Azure portalında bölümü).

- **Önleme modu** isteği bir kuralla eşleşirse önleme modunda çalışacak şekilde yapılandırıldığında, WAF belirtilen eylemi gerçekleştirir. Eşleşen tüm istekleri de WAF günlüklerine kaydedilir.

## <a name="waf-response-for-blocked-requests"></a>WAF yanıt Engellenen istekler için

Varsayılan olarak, WAF eşleşen kural nedeniyle, bir istek engellediğinde bir 403 durum kodu ile - döndürür **istek engellendi** ileti. Bir başvuru dizesi için günlük kaydını da döndürülür.

Bir isteği WAF tarafından engellendiğinde, bir özel yanıt durum kodu ve yanıt iletisi tanımlayabilirsiniz. Aşağıdaki özel durum kodları desteklenir:

- 200 TAMAM
- 403 Yasak
- 405 Yönteme izin verilmiyor
- 406 değil kabul edilebilir
- 429 çok fazla istek

Özel yanıtı durum kodu ve yanıt iletisi bir ilke düzeyi ayarıdır. Yapılandırıldıktan sonra tüm engellenen isteklerin aynı özel yanıt durumu ve yanıt iletisi alın.

## <a name="uri-for-redirect-action"></a>Eylem yeniden yönlendirme URI'si

IF için yeniden yönlendirme istekleri için bir URI tanımlar için gerekli olan **yeniden yönlendirme** herhangi bir WAF ilkesinde yer alan kurallar için seçili eylem. Bu yeniden yönlendirme URI'si geçerli bir HTTP (S) site olması gerekir ve yapılandırıldıktan sonra tüm istekleri "Yeniden yönlendirme" eylemi ile eşleşen kural belirtilen siteye yönlendirileceksiniz.


## <a name="next-steps"></a>Sonraki adımlar
- WAF tanımlamayı öğrenin [özel yanıtlar](waf-front-door-configure-custom-response-code.md)
