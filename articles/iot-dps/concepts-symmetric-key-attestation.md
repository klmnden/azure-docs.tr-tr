---
title: Azure IOT Hub cihazı sağlama hizmeti - simetrik anahtar kanıtı
description: Bu makalede, IOT cihaz sağlama Hizmeti'ni kullanarak simetrik anahtar kanıtı kavramsal bir genel bakış sağlar.
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 2f6e1e1a27e32e567cf0eaa8ff7a99046ed81bbe
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60746257"
---
# <a name="symmetric-key-attestation"></a>Simetrik anahtar kanıtlama

Bu makalede, simetrik anahtarlar cihaz sağlama hizmeti ile kullanırken kimlik kanıtlama işlemi açıklanmaktadır. 

Simetrik anahtar kanıtı, cihaz sağlama hizmeti örneğine sahip bir cihaz kimlik doğrulaması için basit bir yaklaşımdır. Bu kanıtlama yöntemi, cihaz sağlama için yenidir veya katı güvenlik gereksinimleri olmayan geliştiriciler için bir "Hello world" deneyimi temsil eder. Cihaz kanıtlama kullanarak bir [TPM](concepts-tpm-attestation.md) veya [X.509 sertifikası](concepts-security.md#x509-certificates) daha güvenli ve daha katı güvenlik gereksinimleri için kullanılmalıdır.

Simetrik anahtar kayıtları eski cihazları için harika bir yol aracılığıyla Azure IOT bulutuna bootstrap için sınırlı bir güvenlik işlevselliği de sağlar. Simetrik anahtar kanıtı eski cihazları hakkında daha fazla bilgi için bkz. [simetrik anahtarlar eski cihazlarla kullanım nasıl](how-to-legacy-device-symm-key.md).


## <a name="symmetric-key-creation"></a>Simetrik anahtar oluşturma

Yeni kayıtlar ile kaydedildiğinde, varsayılan olarak, cihaz sağlama hizmeti yeni simetrik anahtarlar varsayılan uzunluğu 32 bayt ile oluşturur **anahtarları otomatik olarak oluşturmak** seçeneği etkin.

![Simetrik anahtarları otomatik olarak oluştur](./media/concepts-symmetric-key-attestation/auto-generate-keys.png)

Bu seçenek devre dışı bırakarak, kendi simetrik anahtarlar kayıtları için de sağlayabilirsiniz. Kendi simetrik anahtarlar belirtirken anahtarlarınızı anahtar uzunluğu 16 bayt ve 64 bayt arasında olmalıdır. Ayrıca, simetrik anahtarlar geçerli Base64 biçiminde sağlanmalıdır.



## <a name="detailed-attestation-process"></a>Ayrıntılı kanıtlama işlem

Simetrik anahtar kanıtı cihaz sağlama hizmeti ile aynı kullanarak gerçekleştirilir [güvenlik belirteçleri](../iot-hub/iot-hub-devguide-security.md#security-token-structure) cihazları tanımlamak için IOT hub'ları tarafından desteklenen. Bu güvenlik belirteçleri [paylaşılan erişim imzası (SAS) belirteçleri](../service-bus-messaging/service-bus-sas.md). 

SAS belirteçleri sahip bir karma *imza* simetrik anahtarı kullanılarak oluşturulur. İmza, cihaz sağlama kanıtlama sırasında sunulan bir güvenlik belirteci gerçek olup olmadığını doğrulamak için hizmet tarafından yeniden oluşturulur.

SAS belirteçleri, aşağıdaki biçime sahiptir:

`SharedAccessSignature sig={signature}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Her belirteç bileşenleri şunlardır:

| Değer | Açıklama |
| --- | --- |
| {imzası} |SHA256 HMAC imzası dize. Bireysel kayıtlar için bu imza karma gerçekleştirmek için simetrik anahtarı (birincil veya ikincil) kullanılarak oluşturulur. Kayıt grupları için kayıt grubu anahtarından türetilen bir anahtar karması gerçekleştirmek için kullanılır. Karma formun ileti gerçekleştirilir: `URL-encoded-resourceURI + "\n" + expiry`. **Önemli**: Anahtar base64 kodlaması HMAC SHA256 hesaplama gerçekleştirmek için kullanılmadan önce çözülmüş gerekir. Ayrıca, imza sonuç URL olarak kodlanmış olmalıdır. |
| {resourceURI} |Kapsam kimliği için cihaz sağlama hizmeti örneği başlayarak bu belirteç ile erişilebilir kayıt uç noktası URI'si. Örneğin, `{Scope ID}/registrations/{Registration ID}` |
| {expiry} |UTF8 dizeleri için dönem 00:00:00 UTC 1 Ocak 1970'ten beri geçen saniye sayısı. |
| {URL olarak kodlanmış ResourceURI} |Küçük kaynak URI'si için URL kodlaması düşük durumda |
| {policyName} |Bu belirteç başvurduğu paylaşılan erişim ilkesi adı. Simetrik anahtar kanıtı sağlanırken kullanılan ilke adı **kayıt**. |

Bir cihaz ile bireysel kayıt doğrulama işleminden sonra cihaz bireysel kayıt girişi tanımlanan simetrik anahtarı karma imzası için SAS belirteci oluşturmak için kullanır.

Bir SAS belirteci oluşturan kod örnekleri için bkz. [güvenlik belirteçleri](../iot-hub/iot-hub-devguide-security.md#security-token-structure).

Simetrik anahtar kanıtı için güvenlik belirteçleri oluşturma, Azure IOT C SDK'sı tarafından desteklenir. Bireysel kayıt ile gerçekleştiğini doğrulamak için Azure IOT C SDK'sını kullanarak bir örnek için bkz [simetrik anahtarlar ile bir sanal cihaz sağlama](quick-create-simulated-device-symm-key.md).


## <a name="group-enrollments"></a>Grup kayıtları

Grup kayıtları için simetrik anahtarları sağlanırken doğrudan cihazlar tarafından kullanılmaz. Bunun yerine türetilmiş cihaz anahtarı kullanarak sağlama için bir kayıt ait cihazlar grubu. 

İlk olarak, bir kayıt grubu ile doğrulama işleminden her cihaz için benzersiz kayıt kimliği tanımlanır. Kayıt Kimliği geçerli karakterler: küçük harf alfasayısal ve kısa çizgi ('-'). Bu kayıt kimliği şunun benzersiz cihaz tanımlayan olmalıdır. Örneğin, eski bir cihaz birçok güvenlik özelliği desteklemiyor olabilir. Eski cihaz, yalnızca bir MAC adresi veya seri numarası, cihaz benzersiz olarak tanımlanabilmesi kullanılabilir olabilir. Bu durumda, MAC adresi ve seri numarasını aşağıdakine benzer bir kayıt kimliği oluşabilir:

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Bu tam bir örnek kullanılır [simetrik anahtarlar kullanarak eski cihazları sağlama](how-to-legacy-device-symm-key.md) makalesi.

Cihaz için bir kayıt kimliği tanımlandıktan sonra simetrik anahtar kayıt grubu için işlem kullanılacak bir [HMAC SHA256](https://wikipedia.org/wiki/HMAC) türetilen cihaz anahtarı üretmek için kayıt kimliği karması. Aşağıdaki C# kod ile karma kayıt Kimliğini gerçekleştirilebilir:

```C#
using System; 
using System.Security.Cryptography; 
using System.Text;  

public static class Utils 
{ 
    public static string ComputeDerivedSymmetricKey(byte[] masterKey, string registrationId) 
    { 
        using (var hmac = new HMACSHA256(masterKey)) 
        { 
            return Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(registrationId))); 
        } 
    } 
} 
```

```C#
String deviceKey = Utils.ComputeDerivedSymmetricKey(Convert.FromBase64String(masterKey), registrationId);
```

Sonuçta elde edilen cihaz anahtarı daha sonra kanıtlama için kullanılacak bir SAS belirteci oluşturmak için kullanılır. Bir kayıt grubundaki her bir cihaz, türetilmiş benzersiz bir anahtar, oluşturulan bir güvenlik belirteci kullanılarak gerçekleştiğini doğrulamak için gereklidir. Kayıt grubu simetrik anahtar kanıtı için doğrudan kullanılamaz.

#### <a name="installation-of-the-derived-device-key"></a>Türetilen cihaz anahtarı yüklemesi

İdeal olarak cihaz anahtarları türetilmiş ve Fabrika yüklü. Bu yöntem, Grup anahtarı cihazın kendisine dağıtılan herhangi bir yazılım hiçbir zaman yer aldığı garanti eder. Cihaz MAC adresi veya seri numarası atandığında, anahtar türetilmiş ve ancak depolamak üretici seçer cihazında eklenmiş.

Cihaz anahtarları bir fabrikada grubu kayıt anahtarı ile her cihaz kayıt kimliği karıştırılarak oluşturulan tablosunu gösteren aşağıdaki diyagramda göz önünde bulundurun (**K**). 

![Bir fabrikadan atanmış cihaz anahtarları](./media/concepts-symmetric-key-attestation/key-diversification.png)

Her bir cihaz kimliği kayıt kimliği ve fabrikada yüklü türetilen cihaz anahtarı tarafından temsil edilir. Cihaz anahtarı hiçbir zaman başka bir konuma kopyalanır ve Grup anahtarı hiçbir zaman bir cihazda depolanır.

Cihaz anahtarları factory'de yüklü değilse bir [donanım güvenlik modülü HSM](concepts-security.md#hardware-security-module) cihaz kimliğini güvenli bir şekilde depolamak için kullanılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Simetrik anahtar kanıtı bir anlayışa sahip olduğunuza göre daha fazla bilgi için aşağıdaki makalelere göz atın:

* [Hızlı Başlangıç: Simetrik anahtarlar ile bir sanal cihaz sağlama](quick-create-simulated-device-symm-key.md)
* [Otomatik sağlama kavramları hakkında bilgi edinin](./concepts-auto-provisioning.md)
* [Otomatik sağlama kullanmaya başlama](./quick-setup-auto-provision.md) 
