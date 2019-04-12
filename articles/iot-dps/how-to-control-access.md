---
title: IOT cihaz sağlama hizmeti güvenlik uç noktaların | Microsoft Docs
description: Kavramlar - arka uç uygulamaları için IOT cihaz sağlama Hizmeti'ne erişimi denetleme. Güvenlik belirteçleri hakkında bilgi içerir.
author: wesmc7777
manager: philmea
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: wesmc
ms.openlocfilehash: 7ff622ceac9c49eda7ba6bca1a8bb3aaabccb816
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59495439"
---
# <a name="control-access-to-azure-iot-hub-device-provisioning-service"></a>Azure IOT Hub cihaz sağlama Hizmeti'ne erişimi denetleme

Bu makalede, IOT cihaz sağlama hizmetinize güvenliğini sağlamak için seçenekleri açıklar. Sağlama hizmeti kullandığı *izinleri* her uç noktasına erişim vermek için. İşlevselliğine dayalı bir hizmet örneği için erişim izinleri sınırlayabilir.

Bu makalede açıklanır:

* Farklı izinler, sağlama hizmetinize erişmek için bir arka uç uygulaması verebilirsiniz.
* Kimlik doğrulama işlemi ve belirteçlere izinleri doğrulamak için kullanır.

### <a name="when-to-use"></a>Kullanılması gereken durumlar

Sağlama hizmet uç noktalarına erişmek için uygun izinlere sahip olmalıdır. Örneğin, bir arka uç uygulaması, güvenlik kimlik bilgileri tarafından Hizmeti'ne gönderilen her bir iletiyle birlikte içeren bir belirteç içermesi gerekir.

## <a name="access-control-and-permissions"></a>Erişim denetimi ve izinleri

Size verebilir [izinleri](#device-provisioning-service-permissions) aşağıdaki yollarla:

* **Paylaşılan erişim Yetkilendirme İlkeleri**. Paylaşılan erişim ilkeleri, herhangi bir birleşimini vermek [izinleri](#device-provisioning-service-permissions). İlkeleri tanımlayabilirsiniz [Azure portalında][lnk-management-portal], kullanarak programlama yoluyla veya [cihaz sağlama hizmeti REST API'lerine][lnk-resource-provider-apis]. Yeni oluşturulan bir sağlama hizmeti, aşağıdaki varsayılan ilkesi vardır:

* **provisioningserviceowner**: İlke tüm izinlere sahip.

> [!NOTE]
> Bkz: [izinleri](#device-provisioning-service-permissions) ayrıntılı bilgi için.

## <a name="authentication"></a>Authentication

Azure IOT Hub cihazı sağlama hizmeti, paylaşılan erişim ilkeleri karşı bir belirteci doğrulayarak uç noktalarına erişimi verir. Simetrik anahtarlar gibi güvenlik kimlik bilgilerini asla kablo üzerinden gönderilir.

> [!NOTE]
> Tüm sağlayıcıları olarak cihaz sağlama hizmeti kaynak sağlayıcısı, Azure aboneliğiniz üzerinden sağlanır [Azure Resource Manager][lnk-azure-resource-manager].

Oluşturun ve güvenlik belirteçleri kullanma hakkında daha fazla bilgi için sonraki bölüme bakın.

Yalnızca desteklenen protokol HTTP'dir ve geçerli bir belirteç içine dahil ederek kimlik doğrulaması uygulayan **yetkilendirme** isteği üstbilgisi.

#### <a name="example"></a>Örnek
```csharp
SharedAccessSignature sr = 
   mydps.azure-devices-provisioning.net&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501&skn=provisioningserviceowner`\
```

> [!NOTE]
> [Azure IOT cihaz sağlama hizmeti SDK'ları] [ lnk-sdks] belirteçleri hizmetine olduğunda otomatik olarak oluşturur.

## <a name="security-tokens"></a>Güvenlik belirteçleri

Cihaz sağlama hizmeti, kablo anahtarları göndermekten kaçınmanız Hizmetleri kimlik doğrulaması için güvenlik belirteçlerini kullanır. Ayrıca, güvenlik belirteçleri geçerlilik tarihi ve kapsam bakımından sınırlıdır. [Azure IOT cihaz sağlama hizmeti SDK'ları] [ lnk-sdks] otomatik olarak özel bir yapılandırma gerektirmeden belirteçleri oluşturun. Bazı senaryolar oluşturur ve güvenlik belirteçlerini doğrudan kullanmanızı gerektirir. Bu senaryolara HTTP yüzey doğrudan kullanımını içerir.

### <a name="security-token-structure"></a>Güvenlik belirteci yapısı

IOT cihaz sağlama hizmetinde belirli işlevleri için hizmetler için zaman sınırlı erişim vermek için güvenlik belirteçleri kullanın. Hizmetleri sağlama hizmetine bağlanmak için yetkilendirme almak için bir paylaşılan erişim veya simetrik anahtar ile imzalanmış güvenlik belirteçleri göndermeniz gerekir.

Paylaşılan Erişim İlkesi izinleri ile ilişkili tüm işlevlere bir paylaşılan erişim anahtarı verir erişimi olan bir belirteci imzalayan. 

Güvenlik belirteci biçimi aşağıdaki gibidir:

`SharedAccessSignature sig={signature}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Beklenen değerler şunlardır:

| Değer | Açıklama |
| --- | --- |
| {imzası} |Bir formun SHA256 HMAC imzası dizesi: `{URL-encoded-resourceURI} + "\n" + expiry`. **Önemli**: Anahtar base64 kodlaması çözülmüş ve HMAC SHA256 hesaplama gerçekleştirmek için anahtar olarak kullanılan.|
| {expiry} |UTF8 dizeleri için dönem 00:00:00 UTC 1 Ocak 1970'ten beri geçen saniye sayısı. |
| {URL olarak kodlanmış ResourceURI} | Düşük küçük kaynak URI'si için URL kodlaması durumda. IOT cihaz sağlama hizmeti (hiçbir Protokolü) ana bilgisayar adı ile başlayarak, bu belirteç ile erişilen uç noktalar (segmente) öneki URI. Örneğin, `mydps.azure-devices-provisioning.net`. |
| {policyName} |Bu belirteç başvurduğu paylaşılan erişim ilkesi adı. |

**Önek dekontunda**: URI öneki segmente ve karakter tarafından hesaplanır. Örneğin `/a/b` için bir önek `/a/b/c` ancak `/a/bc`.

Aşağıdaki Node.js kod parçacığında çağrılan bir işlev gösterir **generateSasToken** , girişleri belirteçten hesaplar `resourceUri, signingKey, policyName, expiresInMins`. Farklı girdiler için farklı bir belirteç kullanım durumlarını nasıl sonraki bölümlerde ayrıntılı olarak açıklanmaktadır.

```javascript
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires + "&skn="+ policyName;
    return token;
};
```

Bir karşılaştırma bir güvenlik belirteci oluşturmak için eşdeğer Python kodu verilmiştir:

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl)),
        'skn' : policy_name
    }

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> IOT cihaz sağlama hizmeti makinelerde belirteç süre geçerliliği doğrulanır olduğundan, belirteci oluşturan makinenin saatindeki kayması en az olmalıdır.

### <a name="use-security-tokens-from-service-components"></a>Hizmet bileşenleri güvenlik belirteçleri kullanma

Hizmet bileşenleri, yalnızca güvenlik belirteçleri kullanarak daha önce açıklandığı gibi uygun izinleri verip paylaşılan erişim ilkeleri oluşturabilirsiniz.

Bitiş noktası kullanıma sunulan hizmet işlevleri şunlardır:

| Uç Nokta | İşlev |
| --- | --- |
| `{your-service}.azure-devices-provisioning.net/enrollments` |Cihaz sağlama hizmeti ile cihaz kaydı işlemleri sağlar. |
| `{your-service}.azure-devices-provisioning.net/enrollmentGroups` |Cihaz kayıt grupları yönetmek için işlemler sağlar. |
| `{your-service}.azure-devices-provisioning.net/registrations/{id}` |Alma ve cihaz kayıtlarının durumunu yönetme işlemleri sağlar. |


Örneğin, önceden oluşturulmuş kullanılarak oluşturulan bir hizmet paylaşılan erişim ilkesi adlı **enrollmentread** aşağıdaki parametrelerle bir belirteç oluşturur:

* Kaynak URI: `{mydps}.azure-devices-provisioning.net`,
* imzalama anahtarı: anahtarlarından `enrollmentread` İlkesi
* İlke adı: `enrollmentread`,
* herhangi bir sona erme time.backn

![Portalda bir cihaz sağlama hizmeti örneğinizi için paylaşılan erişim ilkesi oluşturma][img-add-shared-access-policy]

```javascript
var endpoint ="mydps.azure-devices-provisioning.net";
var policyName = 'enrollmentread'; 
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Tüm kayıt kayıtları okuma erişimi vermeniz, sonuç şu şekilde olur:

`SharedAccessSignature sr=mydps.azure-devices-provisioning.net&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=enrollmentread`

## <a name="reference-topics"></a>Başvuru konuları:

Aşağıdaki başvuru konuları, IOT cihaz sağlama Hizmeti'ne erişimi denetleme hakkında daha fazla bilgi sağlar.

### <a name="device-provisioning-service-permissions"></a>Cihaz sağlama hizmeti izinleri

Aşağıdaki tabloda, IOT cihaz sağlama Hizmeti'ne erişimi denetlemek için kullanabileceğiniz izinleri listeler.

| İzin | Notlar |
| --- | --- |
| **ServiceConfig** |Hizmet yapılandırmalarını değiştirme erişimi verir. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **EnrollmentRead** |Okuma cihaz kayıtlarını ve kayıt grupları erişim verir. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **EnrollmentWrite** |Cihaz kayıtlarını ve kayıt grupları için yazma erişimi verir. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **RegistrationStatusRead** |Erişimi cihaz kayıt durumu okuma verir. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **RegistrationStatusWrite**  |Cihaz kayıt durumu için erişim verir silin. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |

<!-- links and images -->

[img-add-shared-access-policy]: ./media/how-to-control-access/how-to-add-shared-access-policy.PNG
[lnk-sdks]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-management-portal]: https://portal.azure.com
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iot-dps/
