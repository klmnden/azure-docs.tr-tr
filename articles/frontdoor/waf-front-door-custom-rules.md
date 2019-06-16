---
title: Web uygulaması güvenlik duvarı özel kuralı için Azure ön kapısı
description: Web uygulaması Güvenlik Duvarı (WAF) özel kurallarını web uygulamalarınızı kötü amaçlı saldırılara karşı koruma kullanmayı öğrenin.
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/07/2019
ms.author: kumud;tyao
ms.openlocfilehash: 744c6fb9235c9daa2d5239ef9fd13679db943650
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61459717"
---
#  <a name="custom-rules-for-web-application-firewall-with-azure-front-door"></a>Web uygulaması güvenlik duvarı ile Azure ön kapı için özel kurallar
Ön kapısı hizmeti ile Azure web uygulaması Güvenlik Duvarı (WAF), web uygulamalarınızı tanımladığınız koşullara göre erişim denetlemenize olanak tanır. Özel bir WAF kural öncelik numarası, kural türü, eşleştirme koşulları ve bir eylem oluşur. Özel kurallar iki tür vardır: eşleşecek kurallar ve hız sınırı kuralları. Bir eşleşme kuralı oranı sınırı kural koşulları ve gelen istekleri fiyatlarına eşleşmesi temeline göre erişim denetlerken koşullar eşleşmesi temeline göre erişimi denetler. Değerlendirilen gelen engellemek için özel bir kural devre dışı ancak yine de yapılandırmayı tutun. Bu makalede, http parametrelerine göre eşleştirme kuralları açıklanır.

## <a name="priority-match-conditions-and-action-types"></a>Öncelik, eşleştirme koşulları ve eylem türleri
Öncelik numarası, kural türü, eşleştirme koşulları ve bir eylemi tanımlayan özel bir WAf kural erişimle denetleyebilirsiniz. 

- **Önceliği:** WAF kural Değerlendirme sırasını tanımlar benzersiz bir tamsayıdır. Kuralları ile düşük değerler daha yüksek değerlerle kurallardan önce değerlendirilir

- **Eylem:** bir WAF kural karşılarsa, bir isteği yönlendirmek nasıl tanımlar. Aşağıdakilerden birini seçebilirsiniz ne zaman uygulamak için eylemleri bir isteği bir özel kural eşleşir.

    - *İzin* -WAF isteği arka uca iletir, bir giriş kaydeder WAF günlükleri ve çıkış yapar.
    - *Blok* -istek engellendi, WAF, arka uç iletilmeden olmadan istemciye yanıt gönderir. WAF bir giriş WAF günlüklerine kaydeder.
    - *Günlük* -WAF bir giriş kaydeder ve devam WAF günlükleri sonraki kural değerlendirin.
    - *Yeniden yönlendirme* -WAF isteği belirtilen URI'ye yeniden yönlendirir, bir giriş WAF günlüklerine kaydeder ve çıkar.

- **Koşulu:** bir işleç bir eşleşme değişkeni tanımlar ve eşleşecek değer. Her kural, birden çok eşleşme koşulu içerebilir. Bir eşleşme koşulu temel aşağıda *eşleşen değişkenler*:
    - RemoteAddr (istemci IP adresi)
    - requestMethod
    - QueryString
    - PostArgs
    - requestUri
    - RequestHeader
    - Includesearchresults: true

- **İşleci:** liste aşağıdakileri içerir:
    - Tüm: genellikle hiçbir kural eşleşirse, varsayılan eylem tanımlamak için kullanılır. Herhangi bir eşleşme tüm işlecidir.
    - IPMatch: IP kısıtlaması RemoteAddr değişkeni tanımlayın
    - GeoMatch: Coğrafi filtreleme için RemoteAddr değişken tanımlayın
    - eşittir
    - İçerir
    - LessThan: boyutu kısıtlaması
    - GreaterThan: boyutu kısıtlaması
    - LessThanOrEqual: boyutu kısıtlaması
    - GreaterThanOrEqual: boyutu kısıtlaması
    - BeginsWith
     - endsWith

Ayarlayabileceğiniz *negate* koşulu koşul sonucu negatif ise true olması.

*Değeriyle eşleşen* olası eşleşme değerlerin listesini tanımlar.
HTTP istek yöntemi değerler desteklenir:
- GET
- POST
- PUT
- HEAD
- DELETE
- KİLİT
- KİLİT AÇMA
- PROFİLİ
- SEÇENEKLER
- PROPFIND
- PROPPATCH
- MKCOL
- KOPYALAMA
- TAŞIMA

## <a name="examples"></a>Örnekler

### <a name="waf-custom-rules-example-based-on-http-parameters"></a>WAF özel kurallar örnek http parametrelerine göre

İki eşleştirme koşulları ile özel bir kural yapılandırmasını gösteren bir örnek aşağıda verilmiştir. İstekleri belirli bir siteden başvuran tarafından tanımlanır ve sorgu dizesi "password" içermiyor.

```
# http rules example
{
  "name": "AllowFromTrustedSites",
  "priority": 1,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestHeader",
      "selector": "Referer",
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "www.mytrustedsites.com/referpage.html"
      ]
    },
    {
      "matchVariable": "QueryString",
      "operator": "Contains",
      "matchValue": [
        "password"
      ],
      "negateCondition": true
    }
  ],
  "action": "Allow",
  "transforms": []
}

```
"PUT" yöntemi engellemeye yönelik örnek bir yapılandırma gösterildiği gibi aşağıdaki:

``` 
# http Request Method custom rules
{
  "name": "BlockPUT",
  "priority": 2,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestMethod",
      "selector": null,
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "PUT"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

### <a name="size-constraint"></a>Boyutu kısıtlaması

Gelen bir isteğin parçası boyutu kısıtlaması belirten özel bir kural oluşturabilirsiniz. Örneğin, kural 100 karakterden daha uzun bir Url engeller.

```
# http parameters size constraint
{
  "name": "URLOver100",
  "priority": 5,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestUri",
      "selector": null,
      "operator": "GreaterThanOrEqual",
      "negateCondition": false,
      "matchValue": [
        "100"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [web uygulaması güvenlik duvarı](waf-overview.md)
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.

