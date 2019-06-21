---
title: Azure Web uygulaması Güvenlik Duvarı (WAF) v2 özel kurallar
description: Bu makalede, Azure Application Gateway Web uygulaması Güvenlik Duvarı (WAF) v2 özel kuralları için genel bir bakış sağlar.
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: f6ea831771a8ffecfdd4c7c0d6374c16894e25ed
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164654"
---
# <a name="custom-rules-for-web-application-firewall-v2"></a>Web uygulaması güvenlik duvarı v2 için özel kurallar

Azure Application Gateway Web uygulaması Güvenlik Duvarı (WAF) v2, farklı türlerde saldırılarına karşı koruma sağlayan önceden yapılandırılmış, platform tarafından yönetilen bir ruleset ile birlikte gelir. Bu tür saldırıları, siteler betik, SQL ekleme ve diğerleri içerir. WAF yöneticisiyseniz, çekirdek kural genişletmek için kendi kurallar kümesi (CRS) kurallarını yazmak isteyebilirsiniz. Kurallarınızı engellemek veya temel ölçütlerle eşleşen üzerinde istenen trafiğe izin verecek.

Özel kurallar, WAF geçirir her istek için değerlendirilen kendi kurallar oluşturmanıza olanak sağlar. Bu kurallar, yönetilen kural kümelerini kuralları geri kalanı daha yüksek bir önceliğe basılı tutun. Kural adı, kural öncelik ve koşullar eşleşen bir dizi özel kurallar içerir. Bu koşullar karşılanıyorsa, bir eylem (izin vermek veya engellemek için) alınır.

Örneğin, bir IP adresi aralığı 192.168.5.4/24 gelen tüm isteklerin engelleyebilirsiniz. Bu kural içinde işleç olarak *IPMatch*matchValues IP adres aralığı (192.168.5.4/24) olduğu ve trafiği engelleme eylemi değildir. Ayrıca kuralın adı ve önceliğini ayarlayın.

Bu adres, güvenlik ihtiyaçlarınızı daha gelişmiş kurallar yapmak için bileşik mantığı kullanarak özel kurallar destekler. For example, (1 koşul **ve** koşul 2) **veya** koşul 3).  Bu örnek olması durumunda anlamına gelir. koşul 1 **ve** 2 koşul karşılanıyorsa, **veya** 3 koşul karşılanıyorsa, WAF özel bir kuralda belirtilen bir eylem gerçekleştirmeniz gerekir.

Aynı kural içinde farklı eşleşen koşullar her zaman compounded kullanarak **ve**. Örneğin, belirli bir IP adresinden gelen trafiği engellemek ve yalnızca belirli bir tarayıcı kullanıyorsanız.

İsterseniz **veya** iki farklı koşullar iki koşul farklı kuralları olmalıdır. Örneğin, belirli bir tarayıcı kullanıyorsanız belirli bir IP adresi ya da blok trafiği gelen trafiği engeller.

> [!NOTE]
> WAF özel kuralları sayısı 100'dür. Application Gateway sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md#application-gateway-limits).

Normal ifadeler de özel kuralları'nda olduğu gibi CRS rulesets desteklenir. Örnek 3. ve 5'te bu örnekler için bkz [özel bir web uygulaması güvenlik duvarı kurallarını oluşturma ve kullanma](create-custom-waf-rules.md).

## <a name="allowing-vs-blocking"></a>Engelleme karşılaştırması izin verme

İzin verme ve trafiği engelleme özel kurallar ile basit bir işlemdir. Örneğin, bir dizi IP adreslerinden gelen tüm trafiği engelleyin. Belirli bir tarayıcıdan isteği geliyorsa, trafiğe izin vermek için başka bir kuralla yapabilirsiniz.

Bir şey izin vermek için emin olun `-Action` parametrenin ayarlanmış **izin**. Bir şey engellemek için emin olun `-Action` parametrenin ayarlanmış **blok**.

```azurepowershell
$AllowRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example1 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Allow

$BlockRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example2 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Block
```

Önceki `$BlockRule` aşağıdaki özel kural Azure Resource Manager'daki eşler:

```json
"customRules": [
      {
        "name": "blockEvilBot",
        "priority": 2,
        "ruleType": "MatchRule",
        "action": "Block",
        "matchConditions": [
          {
            "matchVariables": [
              {
                "variableName": "RequestHeaders",
                "selector": "User-Agent"
              }
            ],
            "operator": "Contains",
            "negationConditon": false,
            "matchValues": [
              "evilbot"
            ],
            "transforms": [
              "Lowercase"
            ]
          }
        ]
      }
    ], 
```

Bu özel kuralın adını, öncelik, eylem ve eylemin gerçekleşmesi için karşılanması gereken koşullar eşleşen bir dizi içeriyor. Daha fazla açıklaması bu alanlar için aşağıdaki alan açıklamalarına bakın. Örneğin özel kuralları Bkz [özel bir web uygulaması güvenlik duvarı kurallarını oluşturma ve kullanma](create-custom-waf-rules.md).

## <a name="fields-for-custom-rules"></a>Alanları için özel kurallar

### <a name="name-optional"></a>[İsteğe bağlı] adı

Bu kural adıdır. Bu ad, günlüklerde görünür.

### <a name="priority-required"></a>Öncelik [gerekli]

- Kuralları değerlendirilir sırasını belirler. Daha düşük değeri, kuralın önceki değerlendirme.
Tüm özel kurallar arasında benzersiz olması gerekir. 100 önceliğine sahip bir kural, 200 önceliğine sahip bir kural önce değerlendirilir.

### <a name="rule-type-required"></a>Kural Türü [gerekli]

Şu anda olmalıdır **MatchRule**.

### <a name="match-variable-required"></a>[Gerekli] eşleşme değişkeni

Değişkenleri biri olmalıdır:

- RemoteAddr – IP adresi/hostname uzak bilgisayar bağlantısı
- RequestMethod – HTTP istek yöntemini (GET, POST, PUT, DELETE vb..)
- Sorgu dizesi – urı'sindeki değişkeni
- PostArgs – bağımsız değişken POST gövdesinde gönderilir.
- RequestUri – isteğin URI'si
- RequestHeaders – istek üst bilgileri
- Includesearchresults: true – istek gövdesi
- RequestCookies – istek tanımlama bilgileri

### <a name="selector-optional"></a>[İsteğe bağlı] Seçici

Alan matchVariable koleksiyonun açıklar. MatchVariable RequestHeaders ise, örneğin, seçici olabilir *User-Agent* başlığı.

### <a name="operator-required"></a>[Gerekli] işleci

Aşağıdaki işleçleri biri olmalıdır:

- Eşleşme değişken olduğunda, yalnızca kullanılan IPMatch - *RemoteAddr*
- Equals – giriş aynıdır MatchValue
- İçerir
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- endsWith
- Regex

### <a name="negate-condition-optional"></a>Negate koşul [isteğe bağlı]

Geçerli koşulu olumsuz duruma getirir.

### <a name="transform-optional"></a>[İsteğe bağlı] dönüştürme

Eşleştirmeden önceki yapmak için dönüşümleri adlarıyla dizelerinin listesini denenir. Bunlar, aşağıdaki dönüştürmeleri olabilir:

- Küçük
- Kırpma
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Eşleşme değerleri [gerekli]

Hangi olarak zorlayıcı olabilir, eşleştirilecek değer listesi *veya*' ed. Örneğin, IP adresleri veya diğer dizeleri olabilir. Değeri biçimi önceki işlecinin bağlıdır.

### <a name="action-required"></a>[Gerekli] eylemi

- İzin ver: tüm sonraki kurallar atlanıyor işlemin yetkisi verir. Başka bir deyişle, belirtilen istek için izin verilenler ve eşleşen, isteği durdurur daha fazla değerlendirme ve arka uç havuzuna gönderilen sonra eklenir. İzin verilenler listesinde kuralları, herhangi bir başka özel kurallar veya yönetilen kuralları için değerlendirilen değildir.
- Blok – engeller göre işlem *SecDefaultAction* (algılama/önleme modu). İzin verme eyleminden gibi istek değerlendirilir ve engelleme listesine eklenmiş sonra değerlendirme durduruldu ve istek engellendi. Aynı koşulları karşılayan sonra herhangi bir istek değerlendirilmeyecektir ve hemen engellenir. 
- Günlük – günlüğüne yazma kuralı izin verir, ancak değerlendirme için rest kurallar sağlar. Sonraki özel kurallar yönetilen kuralları tarafından izlenen, öncelik sırasına göre değerlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

Özel kurallar hakkında bilgi edinin sonra [kendi özel kurallarınızı oluşturmak](create-custom-waf-rules.md).
