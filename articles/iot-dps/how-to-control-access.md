---
title: "IOT cihaz sağlama hizmetindeki güvenlik uç noktaları | Microsoft Docs"
description: "Kavramlar - arka uç uygulamalar için IOT cihaz sağlama hizmete erişimi denetleme. Güvenlik belirteçleri hakkında bilgi içerir."
services: iot-dps
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-dps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: dkshir,rajeevmv
ms.openlocfilehash: 7e98df582baeb4a15b772351802c63fd90303c77
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="control-access-to-azure-iot-hub-device-provisioning-service"></a>Azure IOT Hub cihaz sağlama hizmeti erişimi denetleme

Bu makalede, IOT cihaz sağlama hizmetinize güvenliğini sağlamak için seçenekleri açıklar. Sağlama hizmeti kullandığı *izinleri* her bitiş erişim vermek için. İzinleri işlevselliğine bağlı bir hizmet örneği erişimi sınırlayın.

Bu makalede açıklanır:

* Farklı izinler sağlama hizmetinize erişmek için bir arka uç uygulaması verebilirsiniz.
* Kimlik doğrulama işlemi ve belirteçleri izinleri doğrulamak için kullanır.

### <a name="when-to-use"></a>Kullanılması gereken durumlar

Herhangi bir sağlama hizmet uç noktalarına erişmek için uygun izinlere sahip olmalıdır. Örneğin, bir arka uç uygulaması Hizmeti'ne gönderilen her ileti yanı sıra güvenlik kimlik bilgileri içeren bir belirteç eklemeniz gerekir.

## <a name="access-control-and-permissions"></a>Erişim denetimi ve izinleri

Erişim izni verebilir [izinleri](#device-provisioning-service-permissions) aşağıdaki yollarla:

* **Paylaşılan erişim Yetkilendirme İlkeleri**. Paylaşılan erişim ilkeleri, herhangi bir birleşimini vermek [izinleri](#device-provisioning-service-permissions). İlkeleri tanımlayabilirsiniz [Azure portal][lnk-management-portal], veya program aracılığıyla [aygıt sağlama hizmeti REST API'leri] [lnk-resource-sağlayıcısı-API'leri] kullanarak. Yeni oluşturulan bir sağlama hizmeti aşağıdaki varsayılan ilkesi vardır:

  * **provisioningserviceowner**: tüm izinleri ilkesiyle.

> [!NOTE]
> Bkz: [izinleri](#device-provisioning-service-permissions) ayrıntılı bilgi için.

## <a name="authentication"></a>Kimlik Doğrulaması

Azure IOT Hub cihaz sağlama hizmeti, paylaşılan erişim ilkeleri karşı bir belirteç doğrulayarak uç noktaları erişim verir. Simetrik anahtarlar gibi güvenlik kimlik bilgileri hiçbir zaman kablo üzerinden gönderilir.

> [!NOTE]
> Tüm sağlayıcıları gibi cihaz sağlama hizmeti kaynak sağlayıcısı, Azure aboneliğinizin güvenli [Azure Resource Manager][lnk-azure-resource-manager].

Güvenlik belirteçleri oluşturmak ve nasıl kullanılacağını hakkında daha fazla bilgi için sonraki bölüme bakın.

Yalnızca desteklenen protokol HTTP'dir ve geçerli bir belirteç içine ekleyerek kimlik doğrulaması uygulayan **yetkilendirme** istek üstbilgisi.

#### <a name="example"></a>Örnek
`SharedAccessSignature sr=mydps.azure-devices-provisioning.net&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501&skn=provisioningserviceowner`

> [!NOTE]
> [Azure IOT cihaz sağlama hizmeti SDK'ları] [ lnk-sdks] belirteçleri hizmete bağlanmada olduğunda otomatik olarak oluşturur.

## <a name="security-tokens"></a>Güvenlik belirteçleri
Cihaz sağlama hizmet anahtarları kablo göndermekten kaçınmanız Hizmetleri kimlik doğrulaması için güvenlik belirteçlerini kullanır. Ayrıca, güvenlik belirteçlerini geçerlilik tarihi ve kapsam sınırlıdır. [Azure IOT cihaz sağlama hizmeti SDK'ları] [ lnk-sdks] otomatik olarak özel bir yapılandırma gerektirmeden belirteçleri oluşturur. Bazı senaryolar oluşturmak ve güvenlik belirteçlerini doğrudan kullanmanızı gerektirir. Bu tür senaryoların HTTP yüzeyini doğrudan kullanımını içerir.

### <a name="security-token-structure"></a>Güvenlik belirteci yapısı

IOT cihaz sağlama hizmetinin belirli işlevleri için hizmetleri zaman sınırlı erişim vermek için güvenlik belirteçleri kullanın. Sağlama Hizmeti'ne bağlanmak için yetkilendirme almak için Hizmetleri paylaşılan erişim veya simetrik anahtarı ile imzalanmış güvenlik belirteçleri göndermeniz gerekir.

Paylaşılan Erişim İlkesi izinleri ile ilişkili tüm işlevlere paylaşılan erişim anahtarı verir erişimi olan bir belirteci imzalayan. 

Güvenlik belirteci biçimi aşağıdaki gibidir:

`SharedAccessSignature sig={signature}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Beklenen değerler şunlardır:

| Değer | Açıklama |
| --- | --- |
| {İmza} |Bir HMAC SHA256 imza dize biçiminde: `{URL-encoded-resourceURI} + "\n" + expiry`. **Önemli**: anahtar base64 kodlaması kodunu çözdü ve HMAC SHA256 hesaplama gerçekleştirmek için anahtar olarak kullanılır.|
| {süre sonu} |UTF8 dizeleri dönemi: 00:00:00 UTC üzerinde 1 Ocak 1970'ten beri geçen saniye sayısı. |
| {URL-kodlanmış-resourceURI} | Düşük küçük kaynak URI'si için URL kodlaması durumda. IOT cihaz sağlama hizmeti (iletişim kuralı) ana bilgisayar adı ile başlayarak, bu belirteci ile erişilen uç noktalar için URI öneki (tarafından kesim). Örneğin, `mydps.azure-devices-provisioning.net`. |
| {policyName} |Bu belirteç başvurduğu paylaşılan erişim ilkesinin adı. |

**Not önek üzerinde**: URI öneki segmente göre ve karakter tarafından hesaplanır. Örneğin `/a/b` için bir önek `/a/b/c` ancak için `/a/bc`.

Aşağıdaki Node.js parçacığını adlı bir işlev gösterir **generateSasToken** girişleri belirtecinden hesaplar `resourceUri, signingKey, policyName, expiresInMins`. Sonraki bölümlerde farklı belirteci kullanım örnekleri için farklı girişleri başlatmak nasıl ayrıntılı olarak açıklanmaktadır.

```nodejs
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

Bir karşılaştırma bir güvenlik belirteci üretmek için eşdeğer Python kodu verilmiştir:

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
> Belirtecin süresi geçerliliğini IOT cihaz hizmeti sağlama makinelerde doğrulanır beri belirteci oluşturur makine saatinde kayması en az olmalıdır.


### <a name="use-security-tokens-from-service-components"></a>Hizmet bileşenleri gelen güvenlik belirteçleri kullanın

Hizmet bileşenleri, yalnızca daha önce açıklandığı gibi uygun izinleri verme paylaşılan erişim ilkeleri kullanan güvenlik belirteçleri üretebilir.

Bitiş noktasında kullanıma sunulan hizmet işlevler şunlardır:

| Uç Nokta | İşlev |
| --- | --- |
| `{your-service}.azure-devices-provisioning.net/enrollments` |Cihaz sağlama hizmeti ile cihaz kayıt işlemler sağlar. |
| `{your-service}.azure-devices-provisioning.net/enrollmentGroups` |Aygıt kayıt gruplarını yönetmek için işlemler sağlar. |
| `{your-service}.azure-devices-provisioning.net/registrations/{id}` |Alma ve cihaz kayıtları durumunu yönetmek için işlemler sağlar. |


Örnek olarak, önceden oluşturulmuş kullanılarak oluşturulan bir hizmet paylaşılan erişim ilkesi adlı **enrollmentread** aşağıdaki parametrelerle bir belirteç oluşturacak:

* Kaynak URI'si: `{mydps}.azure-devices-provisioning.net`,
* imzalama anahtarı: anahtarlarını birini `enrollmentread` İlkesi
* İlke adı: `enrollmentread`,
* Tüm süre sonu zamanı.

![Portalda DPS Örneğiniz için paylaşılan erişim ilkesi oluşturma][img-add-shared-access-policy]

```nodejs
var endpoint ="mydps.azure-devices-provisioning.net";
var policyName = 'enrollmentread'; 
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Tüm kayıt kayıtları okuma erişimi vermeniz, sonuç şöyle olacaktır:

`SharedAccessSignature sr=mydps.azure-devices-provisioning.net&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=enrollmentread`

## <a name="reference-topics"></a>Başvuru konuları:

Aşağıdaki başvuru konuları IOT cihaz sağlama hizmetinizi erişimini denetleme hakkında daha fazla bilgi sağlar.

## <a name="device-provisioning-service-permissions"></a>Cihaz sağlama hizmet izinleri

Aşağıdaki tabloda, IOT cihaz hizmeti sağlama erişimi denetlemek için kullanabileceğiniz izinleri listeler.

| İzin | Notlar |
| --- | --- |
| **ServiceConfig** |Hizmet yapılandırması değiştirmek için verir erişin. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **EnrollmentRead** |Cihaz kayıtlarını ve kayıt gruplara erişim okuma verir. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **EnrollmentWrite** |Cihaz kayıtlarını ve kayıt gruplarına yazma erişimi verir. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **RegistrationStatusRead** |Okuma erişimi cihaz kayıt durumunu verir. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **RegistrationStatusWrite**  |Cihaz kayıt durumu erişim verir silin. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |

<!-- links and images -->

[img-add-shared-access-policy]: ./media/how-to-control-access/how-to-add-shared-access-policy.PNG
[lnk-sdks]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-management-portal]: https://portal.azure.com
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
