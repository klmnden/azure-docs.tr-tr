---
title: Azure IOT Hub güvenliği anlama | Microsoft Docs
description: Geliştirici Kılavuzu - cihaz uygulamaları ve arka uç uygulamalar için IOT Hub'ına erişimi denetleme. Güvenlik belirteçleri ve X.509 sertifikalarını desteği hakkında bilgiler içerir.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 45631e70-865b-4e06-bb1d-aae1175a52ba
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/12/2018
ms.author: dobett
ms.openlocfilehash: c410db9a7255a039ab9b41ae39f2fe1018719f8f
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="control-access-to-iot-hub"></a>IoT Hub’a erişimi denetleme

Bu makalede, IOT hub'ınızı güvenliğini sağlamak için seçenekleri açıklar. IOT hub'ı kullanan *izinleri* her IOT hub uç erişim vermek için. İzinleri işlevselliğine dayalı bir IOT hub'ına erişimi sınırlayın.

Bu makalede sunar:

* IOT hub'ınızı erişmek için bir aygıt veya arka uç uygulaması için erişim izni verebilir farklı izinler.
* Kimlik doğrulama işlemi ve belirteçleri izinleri doğrulamak için kullanır.
* Belirli kaynaklara erişimi sınırlamak için kimlik bilgilerini kapsam yapma.
* X.509 sertifikaları IOT hub'ı destekler.
* Var olan cihaz kimlik kayıt defterleri veya kimlik doğrulama şemasını kullanan özel cihaz kimlik doğrulama mekanizmaları.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IOT Hub uç noktaları hiçbirini erişmek için uygun izinlere sahip olmalıdır. Örneğin, bir cihaz IOT Hub'ına gönderir her ileti yanı sıra güvenlik kimlik bilgileri içeren bir belirteç eklemeniz gerekir.

## <a name="access-control-and-permissions"></a>Erişim denetimi ve izinleri

Erişim izni verebilir [izinleri](#iot-hub-permissions) aşağıdaki yollarla:

* **IOT hub düzeyi paylaşılan erişim ilkeleri**. Paylaşılan erişim ilkeleri, herhangi bir birleşimini vermek [izinleri](#iot-hub-permissions). İlkeleri tanımlayabilirsiniz [Azure portal][lnk-management-portal], veya program aracılığıyla kullanarak [IOT hub'ı kaynak sağlayıcısı REST API'leri][lnk-resource-provider-apis]. Yeni oluşturulan bir IOT hub aşağıdaki varsayılan ilkeleri vardır:

  * **iothubowner**: tüm izinleri ilkesiyle.
  * **Hizmet**: ilke ile **ServiceConnect** izni.
  * **Aygıt**: ilke ile **DeviceConnect** izni.
  * **registryRead**: ilke ile **RegistryRead** izni.
  * **registryReadWrite**: ilke ile **RegistryRead** ve RegistryWrite izinleri.
  * **Cihaz başına güvenlik kimlik bilgileri**. Her IOT hub'ı içeren bir [kimlik kayıt defteri][lnk-identity-registry]. Bu kimlik kayıt defterinde her cihaz için vermek güvenlik kimlik bilgileri yapılandırabilirsiniz **DeviceConnect** karşılık gelen cihaz Uç noktalara kapsamlı izinleri.

Örneğin, tipik bir IOT çözüm içinde:

* Aygıt Yönetimi bileşeni kullanır *registryReadWrite* ilkesi.
* Olay işlemcisi bileşeni kullanır *hizmet* ilkesi.
* Çalışma zamanı aygıt iş mantığı bileşeni kullanır *hizmet* ilkesi.
* Tek bir cihazı IOT hub'ın kimlik kayıt defterinde depolanan kimlik bilgilerini kullanarak bağlanın.

> [!NOTE]
> Bkz: [izinleri](#iot-hub-permissions) ayrıntılı bilgi için.

## <a name="authentication"></a>Kimlik Doğrulaması

Azure IOT Hub kimlik kayıt defteri güvenlik kimlik bilgileri ve paylaşılan erişim ilkeleri karşı bir belirteç doğrulayarak uç noktaları erişim verir.

Simetrik anahtarlar gibi güvenlik kimlik bilgileri hiçbir zaman kablo üzerinden gönderilir.

> [!NOTE]
> Tüm sağlayıcıları olarak Azure IOT Hub kaynak sağlayıcısı, Azure aboneliğinizin güvenli [Azure Resource Manager][lnk-azure-resource-manager].

Güvenlik belirteçleri oluşturmak ve nasıl kullanılacağını hakkında daha fazla bilgi için bkz: [IOT hub'ı güvenlik belirteçleri][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokol özellikleri

Her MQTT, AMQP ve HTTPS gibi desteklenen bir protokol belirteçleri farklı şekillerde taşımaları.

MQTT kullanırken, istemci kimliği olarak DeviceID BAĞLAN paket sahip `{iothubhostname}/{deviceId}` Username alanı ve parola alanına bir SAS belirteci. `{iothubhostname}` IOT hub'ı (örneğin, contoso.azure-devices.net) tam CNAME'i olmalıdır.

Kullanırken [AMQP][lnk-amqp], IOT hub'ı destekleyen [SASL DÜZ] [ lnk-sasl-plain] ve [AMQP talep tabanlı-güvenlik] [ lnk-cbs].

AMQP talep tabanlı-güvenlik kullanırsanız, bu belirteçleri iletmek nasıl standart belirtir.

SASL DÜZ için **kullanıcıadı** olabilir:

* `{policyName}@sas.root.{iothubName}` IOT hub düzeyindeki belirteçleri kullanıyorsanız.
* `{deviceId}@sas.{iothubname}` cihaz kapsamlı belirteçleri kullanıyorsanız.

Parola alanı belirteci açıklandığı gibi her iki durumda da içeren [IOT hub'ı güvenlik belirteçleri][lnk-sas-tokens].

Geçerli bir belirteç içine ekleyerek HTTPS kimlik doğrulaması uygulayan **yetkilendirme** istek üstbilgisi.

#### <a name="example"></a>Örnek

Kullanıcı adı (DeviceID büyük küçük harfe duyarlı): `iothubname.azure-devices.net/DeviceId`

Parola (oluşturmak SAS belirteci ile [aygıt explorer] [ lnk-device-explorer] aracı): `SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> [Azure IOT SDK'ları] [ lnk-sdks] belirteçleri hizmete bağlanmada olduğunda otomatik olarak oluşturur. Bazı durumlarda, tüm protokoller veya tüm kimlik doğrulama yöntemleri Azure IOT SDK'ları desteklemez.

### <a name="special-considerations-for-sasl-plain"></a>SASL DÜZ için özel hususlar

SASL DÜZ AMQP ile kullanırken, bir IOT hub'ına bağlanan bir istemci her TCP bağlantısı için tek bir belirteci kullanabilirsiniz. Belirtecin süresi dolduğunda, TCP bağlantısı hizmet bağlantısını keser ve bir yeniden bağlanma tetikler. Bu davranış değil sorunlu sırada bir arka uç uygulaması için bir cihaz uygulaması için aşağıdaki nedenlerle zarar:

* Ağ geçidi genellikle birçok aygıtlar adına bağlayın. SASL DÜZ kullanırken, bunlar bir IOT hub'ına bağlanan her aygıt için ayrı bir TCP bağlantı oluşturmanız gerekir. Bu senaryoda önemli ölçüde güç ve ağ kaynakları tüketiminin artırır ve her cihaz bağlantı gecikmesi artırır.
* Kaynak kısıtlı cihazları olumsuz her belirteç süresi dolduktan sonra yeniden bağlanmayı kaynak kullanımının yüksek etkilenir.

## <a name="scope-iot-hub-level-credentials"></a>Kapsam IOT hub düzeyindeki kimlik bilgileri

Sınırlı kaynak URI'si ile belirteçleri oluşturarak IOT hub'ı düzeyinde güvenlik ilkelerinin kapsamını belirleyebilirsiniz. Örneğin, bir CİHAZDAN cihaz bulut iletilerini göndermek için uç nokta **/devices/ {DeviceID} / iletileri/olayları**. Bir IOT hub düzeyindeki paylaşılan erişim ilkesi ile de kullanabileceğiniz **DeviceConnect** , resourceURI olan bir belirteç imzalamak için izinleri **/devices/ {DeviceID}**. Bu yaklaşım, yalnızca cihaz adına iletileri göndermek için kullanılabilen bir belirteç oluşturur **DeviceID**.

Bu mekanizma benzer [olay hub'ları Yayımcı ilkesi][lnk-event-hubs-publisher-policy]ve özel kimlik doğrulama yöntemleri sağlar.

## <a name="security-tokens"></a>Güvenlik belirteçleri

IOT Hub, aygıtları ve kablo anahtarları göndermekten kaçınmanız Hizmetleri kimlik doğrulaması için güvenlik belirteçlerini kullanır. Ayrıca, güvenlik belirteçlerini geçerlilik tarihi ve kapsam sınırlıdır. [Azure IOT SDK'ları] [ lnk-sdks] otomatik olarak özel bir yapılandırma gerektirmeden belirteçleri oluşturur. Bazı senaryolar oluşturmak ve güvenlik belirteçlerini doğrudan kullanmanızı gerektirir. Bu tür senaryoları şunları içerir:

* MQTT, AMQP veya HTTPS yüzeyleri doğrudan kullanın.
* Açıklandığı gibi belirteci hizmeti düzeni uyarlamasını [özel cihaz kimlik doğrulamasını][lnk-custom-auth].

IOT Hub ayrıca kullanarak IOT Hub ile kimlik doğrulaması cihazları sağlar [X.509 sertifikalarını][lnk-x509].

### <a name="security-token-structure"></a>Güvenlik belirteci yapısı

Cihazlara zaman sınırlı erişim ve IOT Hub'ındaki belirli işlevleri hizmet vermek için güvenlik belirteçleri kullanın. IOT Hub'ına bağlanmak için yetkilendirme almak için cihazları ve Hizmetleri paylaşılan erişim veya simetrik anahtarı ile imzalanmış güvenlik belirteçleri göndermeniz gerekir. Bu anahtarlar, kimlik kayıt defterinde bir cihaz kimliği ile depolanır.

Paylaşılan Erişim İlkesi izinleri ile ilişkili tüm işlevlere paylaşılan erişim anahtarı verir erişimi olan bir belirteci imzalayan. Bir belirteci imzalayan aygıt kimliğin simetrik anahtar yalnızca verir ile **DeviceConnect** ilişkili cihaz kimliği için izni.

Güvenlik belirteci biçimi aşağıdaki gibidir:

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Beklenen değerler şunlardır:

| Değer | Açıklama |
| --- | --- |
| {İmza} |Bir HMAC SHA256 imza dize biçiminde: `{URL-encoded-resourceURI} + "\n" + expiry`. **Önemli**: anahtar base64 kodlaması kodunu çözdü ve HMAC SHA256 hesaplama gerçekleştirmek için anahtar olarak kullanılır. |
| {resourceURI} |IOT hub'ı (Protokol) ana bilgisayar adı ile başlayarak, bu belirteci ile erişilen uç noktalar için URI öneki (tarafından kesim). Örneğin, `myHub.azure-devices.net/devices/device1` |
| {expiry} |UTF8 dizeleri dönemi: 00:00:00 UTC üzerinde 1 Ocak 1970'ten beri geçen saniye sayısı. |
| {URL-encoded-resourceURI} |Düşük küçük kaynak URI'si için URL kodlaması durumda |
| {policyName} |Bu belirteç başvurduğu paylaşılan erişim ilkesinin adı. Yüklenmesinden belirteç aygıt kayıt defteri kimlik bilgilerini ifade eder. |

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

    // Construct autorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
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
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

Bir güvenlik belirteci üretmek için C# işlevdir:

```csharp
using System;
using System.Globalization;
using System.Net;
using System.Net.Http;
using System.Security.Cryptography;
using System.Text;

public static string generateSasToken(string resourceUri, string key, string policyName, int expiryInSeconds = 3600)
{
    TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
    string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + expiryInSeconds);

    string stringToSign = WebUtility.UrlEncode(resourceUri) + "\n" + expiry;

    HMACSHA256 hmac = new HMACSHA256(Convert.FromBase64String(key));
    string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));

    string token = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}", WebUtility.UrlEncode(resourceUri), WebUtility.UrlEncode(signature), expiry);

    if (!String.IsNullOrEmpty(policyName))
    {
        token += "&skn=" + policyName;
    }

    return token;
}

```


> [!NOTE]
> Belirtecin süresi geçerliliğini IOT hub'ı makinelerde doğrulanır beri belirteci oluşturur makine saatinde kayması en az olmalıdır.

### <a name="use-sas-tokens-in-a-device-app"></a>SAS belirteci bir aygıt uygulamasında kullanma

Almak için iki yolla **DeviceConnect** güvenlik belirteçleri IOT Hub ile izinlerle: kullanın bir [simetrik cihaz kimlik kayıt defteri anahtarından](#use-a-symmetric-key-in-the-identity-registry), veya bir [paylaşılan erişim anahtarı](#use-a-shared-access-policy).

Tüm işlevlere aygıtlardan erişilebilir önekiyle uç noktalarda tasarıma göre sunulur unutmayın `/devices/{deviceId}`.

> [!IMPORTANT]
> IOT hub'ı belirli bir aygıtın kimliğini doğrulayan tek yolu cihazı kimlik simetrik anahtar kullanıyor. Cihazın işlevselliğini erişmek için bir paylaşılan erişim ilkesi kullanıldığında durumlarda güvenilir bir alt bileşen olarak güvenlik belirteci veren bileşen çözümü dikkate almanız gerekir.

Aygıt'e yönelik uç noktalar (yedeklemiş Protokolü) şunlardır:

| Uç Nokta | İşlev |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |Cihaz-bulut iletileri gönderir. |
| `{iot hub host name}/devices/{deviceId}/messages/devicebound` |Bulut-cihaz iletilerini alır. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Simetrik anahtar kimlik kayıt defterinde kullanma

Bir aygıt kimliğin simetrik anahtar policyName bir belirteç oluşturmak için kullanırken (`skn`) öğesi belirtecin atlanmış.

Örneğin, tüm cihaz işlevlerini erişmek için oluşturulan bir belirteç aşağıdaki parametreleri içermelidir:

* Kaynak URI'si: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* imzalama anahtarı: herhangi bir simetrik anahtar için `{device id}` kimliği
* hiçbir ilke adı
* Tüm süre sonu zamanı.

Önceki Node.js işlevini kullanarak bir örnek olabilir:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

Device1 tüm işlevlere erişimi verir, sonuç şöyle olacaktır:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> .NET kullanarak bir SAS belirteci oluşturmak mümkündür [aygıt explorer] [ lnk-device-explorer] aracını veya platformlar arası, Python tabanlı [IOT uzantısı için Azure CLI 2.0] [ lnk-IoT-extension-CLI-2.0] komut satırı yardımcı programı.

### <a name="use-a-shared-access-policy"></a>Bir paylaşılan erişim ilkesi kullanın

Paylaşılan Erişim İlkesi tarafından bir belirteç oluşturduğunuzda `skn` ilkesinin adı alanı. Bu ilke vermelidir **DeviceConnect** izni.

Cihazın işlevselliğini erişmek için paylaşılan erişim ilkeleri kullanarak için iki ana senaryo şunlardır:

* [protokol ağ geçidi bulut][lnk-endpoints],
* [simge Hizmetleri] [ lnk-custom-auth] özel kimlik doğrulama şemasını gerçekleştirmek için kullanılır.

Paylaşılan Erişim İlkesi potansiyel herhangi bir aygıt bağlanmak için erişim izni verebilir, doğru kaynak URI'si güvenlik belirteçleri oluşturmak kullanmak önemlidir. Bu ayar, kaynak URI'si kullanarak belirli bir cihaza belirteci kapsam zorunda belirteci Hizmetleri için özellikle önemlidir. Bu zaten tüm aygıtlar için trafiği eşlemlerse protokolü ağ geçitleri için daha az ilgili noktasıdır.

Örnek olarak, önceden oluşturulmuş kullanarak bir belirteç hizmet paylaşılan erişim ilkesi adlı **aygıt** aşağıdaki parametrelerle bir belirteç oluşturacak:

* Kaynak URI'si: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* imzalama anahtarı: anahtarlarını birini `device` İlkesi
* İlke adı: `device`,
* Tüm süre sonu zamanı.

Önceki Node.js işlevini kullanarak bir örnek olabilir:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Device1 tüm işlevlere erişimi verir, sonuç şöyle olacaktır:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

Bir protokol ağ geçidi yalnızca kaynak URI'si ayarı tüm aygıtlar için aynı belirteci kullanabilirsiniz için `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Hizmet bileşenleri gelen güvenlik belirteçleri kullanın

Hizmet bileşenleri, yalnızca daha önce açıklandığı gibi uygun izinleri verme paylaşılan erişim ilkeleri kullanan güvenlik belirteçleri üretebilir.

Bitiş noktasında kullanıma sunulan hizmet işlevleri şöyledir:

| Uç Nokta | İşlev |
| --- | --- |
| `{iot hub host name}/devices` |Oluştur, Güncelleştir, alabilir ve cihaz kimliklerini silebilir. |
| `{iot hub host name}/messages/events` |Cihaz bulut iletilerini alır. |
| `{iot hub host name}/servicebound/feedback` |Bulut-cihaz iletilerini için bildirim alırsınız. |
| `{iot hub host name}/devicebound` |Bulut cihaz iletileri gönderir. |

Örnek olarak, önceden oluşturulmuş kullanarak bir hizmet oluşturma adlı erişim ilkesi paylaşılan **registryRead** aşağıdaki parametrelerle bir belirteç oluşturacak:

* Kaynak URI'si: `{IoT hub name}.azure-devices.net/devices`,
* imzalama anahtarı: anahtarlarını birini `registryRead` İlkesi
* İlke adı: `registryRead`,
* Tüm süre sonu zamanı.

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Tüm aygıt kimlikleri okuma erişimi vermeniz, sonuç şöyle olacaktır:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a>Desteklenen X.509 sertifikaları

Sertifika parmak izi veya bir sertifika yetkilisi (CA) Azure IOT Hub'ına karşıya yükleyerek bir cihaz IOT Hub ile kimlik doğrulaması için herhangi bir X.509 sertifika kullanabilirsiniz. Yalnızca sertifika parmak izleri kullanılarak kimlik doğrulaması sunulan parmak izi yapılandırılmış parmak iziyle eşleştiğini doğrular. Sertifika yetkilisi kullanılarak kimlik doğrulaması sertifika zincirini doğrular. 

Desteklenen sertifikaları dahil et:

* **Varolan bir X.509 sertifikası**. Bir aygıt zaten kendisiyle ilişkilendirilmiş bir X.509 sertifikası olabilir. Cihaz IOT Hub ile kimlik doğrulaması için bu sertifikayı kullanabilirsiniz. Parmak izi veya CA'ın kimlik doğrulaması ile çalışır. 
* **X.509 sertifikası CA tarafından imzalanmış**. Bir aygıtı belirlemek ve IOT Hub ile kimlik doğrulaması için oluşturulan ve bir sertifika yetkilisi (CA) tarafından imzalanmış bir X.509 sertifikası kullanabilirsiniz. Parmak izi veya CA'ın kimlik doğrulaması ile çalışır.
* **A otomatik olarak oluşturulan ve X-509 Sertifika'otomatik olarak imzalanan**. Bir aygıt üreticisi veya şirket içi dağıtıcı, bu sertifikalar oluşturmak ve karşılık gelen özel anahtar (ve sertifika) cihazda depolamak. Araçları gibi kullanabilir [OpenSSL] [ lnk-openssl] ve [Windows SelfSignedCertificate] [ lnk-selfsigned] bu amaç için yardımcı programı. Yalnızca parmak izi kimlik doğrulaması ile çalışır. 

Bir aygıt ya da bir X.509 sertifikası veya bir güvenlik belirteci kimlik doğrulaması, ancak ikisini için kullanabilir.

Sertifika yetkilisi kullanılarak kimlik doğrulaması hakkında daha fazla bilgi için bkz: [X.509 CA sertifikaları kavramsal anlayış](iot-hub-x509ca-concept.md).

### <a name="register-an-x509-certificate-for-a-device"></a>Bir aygıt bir X.509 sertifikası Kaydet

[Azure IOT hizmeti SDK'sı için C#] [ lnk-service-sdk] (sürüm 1.0.8+) destekleyen bir X.509 sertifikası için kimlik doğrulaması kullanan bir cihazı kaydetme. İçeri/dışarı aktarma aygıtların gibi diğer API'leri X.509 sertifikalarını da destekler.

### <a name="c-support"></a>C\# desteği

**RegistryManager** sınıfı bir cihazı kaydetmek için programlı bir yolunu sağlar. Özellikle, **AddDeviceAsync** ve **UpdateDeviceAsync** yöntemleri kaydetmek ve bir cihaz IOT Hub kimlik kayıt defterinde güncelleştirmenize olanak sağlar. Bu iki yöntem ele bir **aygıt** örnek giriş olarak. **Aygıt** sınıfı içeren bir **kimlik doğrulaması** birincil ve ikincil X.509 sertifika parmak izlerini belirtmenize olanak tanır özelliği. Parmak izi SHA-1 karma (DER ikili kodlama kullanılarak depolanır) X.509 sertifikası değerini temsil eder. Birincil bir parmak izi veya ikincil bir parmak izi veya her ikisini belirtme seçeneğiniz vardır. Birincil ve ikincil parmak izleri sertifika geçiş senaryoları işlemek için desteklenir.

Örnek C işte\# kod parçacığını bir X.509 sertifika parmak izini kullanarak bir cihazı kaydetmek için:

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Çalıştırma işlemleri sırasında bir X.509 sertifikası kullanın

[.NET için Azure IOT cihaz SDK'sı] [ lnk-client-sdk] (sürüm 1.0.11+) X.509 sertifikalarını kullanımını destekler.

### <a name="c-support"></a>C\# desteği

Sınıf **DeviceAuthenticationWithX509Certificate** oluşturmayı destekler **DeviceClient** X.509 sertifikası kullanarak örnekleri. X.509 sertifikasının özel anahtarı içerir (PKCS #12 olarak da bilinir) PFX biçiminde olmalıdır.

Örnek kod parçacığı aşağıda verilmiştir:

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Özel cihaz kimlik doğrulaması

IOT hub'ı kullanabilirsiniz [kimlik kayıt defteri] [ lnk-identity-registry] cihaz başına güvenlik kimlik bilgilerini yapılandırmak ve denetimini kullanarak erişmek için [belirteçleri][lnk-sas-tokens]. Bir IOT çözümündeki bir özel kimlik kayıt defteri ve/veya kimlik doğrulama düzeni zaten varsa, oluşturmayı göz önünde bulundurun bir *belirteç hizmeti* bu altyapı IOT Hub ile tümleştirmek için. Bu şekilde, çözümünüz içinde diğer IOT özelliklerini kullanabilirsiniz.

Bir belirteci hizmeti bir özel bulut hizmetidir. IOT hub'ı kullanan *paylaşılan erişim ilkesi* ile **DeviceConnect** oluşturma izni *aygıt kapsamlı* belirteçleri. Bu belirteçler IOT hub'ınıza bağlanmak bir aygıt etkinleştirin.

![Belirteç Hizmeti deseninin adımları][img-tokenservice]

Belirteç Hizmeti deseninin ana adımlar şunlardır:

1. IOT Hub'ı paylaşılan erişim ilkesi ile oluşturma **DeviceConnect** IOT hub'ınız için izinleri. Bu ilkede oluşturabilirsiniz [Azure portal] [ lnk-management-portal] veya program aracılığıyla. Belirteç hizmetine bu ilkeyi oluşturduğu Belirteçleri imzalamak için kullanır.
1. Bir cihaz IOT hub'ınızı erişim gerektiğinde, imzalı bir belirteç belirteci Hizmeti'nden ister. Cihaz belirteci hizmeti belirteç oluşturmak için kullandığı cihaz kimliğini belirlemek için özel kimlik kayıt defteri/kimlik doğrulama düzeniyle doğrulayabilir.
1. Belirteç hizmetine bir belirteci döndürür. Belirteç kullanılarak oluşturulan `/devices/{deviceId}` olarak `resourceURI`, ile `deviceId` kimlik doğrulaması gerçekleştirilen aygıt olarak. Belirteç hizmetine paylaşılan erişim ilkesi belirteç oluşturmak için kullanılır.
1. Cihaz belirteci doğrudan IOT hub ile kullanır.

> [!NOTE]
> Bir .NET sınıfını kullanabilirsiniz [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] veya Java sınıfı [IotHubServiceSasToken] [ lnk-java-sas] bir belirteç oluşturmak için Belirteç Hizmeti.

Belirteç hizmetine belirteci süre sonu istendiği gibi ayarlayabilirsiniz. Belirtecin süresi dolduğunda, IOT hub cihaz bağlantısı sunucularının. Ardından, cihaz belirteci Hizmeti'nden yeni bir belirteç istemeniz gerekir. Kısa süre sonu zamanı hem cihaz hem de belirteç hizmetine üzerindeki yükü artırır.

Hub'ınıza bağlanmak bir aygıt için hala onu IOT Hub kimlik kayıt defterine eklemeniz gerekir — cihaz bir belirteç ve aygıt anahtarı bağlanmak için kullanıyor olsa da. Bu nedenle, etkinleştirme veya cihaz kimliklerini devre dışı bırakarak aygıt başına erişim denetimi kullanmaya devam edebilirsiniz [kimlik kayıt defteri][lnk-identity-registry]. Bu yaklaşım, uzun süre sonu zamanlarında belirteçleri kullanarak riskleri azaltır.

### <a name="comparison-with-a-custom-gateway"></a>Özel bir ağ geçidi ile karşılaştırma

Belirteç Hizmeti düzeni, IOT Hub ile bir özel kimlik kayıt defteri/kimlik doğrulaması düzeni uygulamak için önerilen yoludur. IOT hub'ı çoğu çözüm trafiği işlemeye devam ettiğinden bu deseni önerilir. Ancak, özel kimlik doğrulama şeması böylece protokolüyle birbirine, gerektirebilecek bir *özel ağ geçidi* tüm trafiğini işlemek için. Böyle bir senaryo örneği kullanarak[Aktarım Katmanı Güvenliği (TLS) ve önceden paylaşılan anahtarı (PSKs)][lnk-tls-psk]. Daha fazla bilgi için bkz: [protokol ağ geçidi] [ lnk-protocols] makalesi.

## <a name="reference-topics"></a>Başvuru konuları:

Aşağıdaki başvuru konuları IOT hub'ınıza erişimini denetleme hakkında daha fazla bilgi sağlar.

## <a name="iot-hub-permissions"></a>IOT hub'ı izinleri

Aşağıdaki tabloda, IOT hub'ınıza erişimi denetlemek için kullanabileceğiniz izinleri listeler.

| İzin | Notlar |
| --- | --- |
| **RegistryRead** |Okuma kimlik kayıt defterine erişim verir. Daha fazla bilgi için bkz: [kimlik kayıt defteri][lnk-identity-registry]. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **RegistryReadWrite** |Verir okuma ve yazma erişimi kimlik kayıt defterine. Daha fazla bilgi için bkz: [kimlik kayıt defteri][lnk-identity-registry]. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **ServiceConnect** |Hizmet dönük iletişim ve uç nokta izleme buluta verir erişin. <br/>Cihaz bulut iletilerini, bulut-cihaz iletilerini göndermek ve karşılık gelen teslim alındı bildirimleri almak için izin verir. <br/>Karşıya yükleme dosyası için teslim bildirimleri almak için izin verir. <br/>Etiketler ve istenen özelliklerini güncelleştirmek için bildirilen özelliklerini almak ve sorguları çalıştırmak için cihaz çiftlerini erişmesine izin verir. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **DeviceConnect** |Aygıt'te Uç noktalara erişimi verir. <br/>Cihaz bulut iletilerini göndermek ve bulut-cihaz iletilerini izni verir. <br/>Bir CİHAZDAN karşıya dosya yükleme gerçekleştirme izni verir. <br/>Cihaz çifti istenen özellik bildirimleri almak ve cihaz çifti güncelleştirme izni verir özellikleri bildirdi. <br/>Verir dosya gerçekleştirme izni yükler. <br/>Bu izin, cihazlar tarafından kullanılır. |

## <a name="additional-reference-material"></a>Ek başvuru bilgileri

IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] her IOT hub'ı çalışma zamanı ve yönetim işlemleri için kullanıma sunan çeşitli uç noktaları açıklar.
* [Azaltma ve kotaları] [ lnk-quotas] kotaları açıklar ve IOT hub'ı hizmete uygulamak davranışları azaltma.
* [Azure IOT cihaz ve hizmet SDK'ları] [ lnk-sdks] çeşitli dil IOT Hub ile etkileşim hem cihaz hem de hizmet uygulamaları geliştirirken kullanabilir SDK'ları listeler.
* [IOT hub'ı sorgu dili] [ lnk-query] IOT Hub'ından, cihaz çiftlerini ve işleri hakkında bilgi almak için kullanabileceğiniz sorgu dili açıklar.
* [IOT Hub MQTT Destek] [ lnk-devguide-mqtt] IOT hub'ı desteği hakkında daha fazla bilgi için MQTT Protokolü sağlar.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı erişimi denetlemek öğrendiniz, aşağıdaki IOT Hub Geliştirici Kılavuzu konuları ilgilenen olabilir:

* [Cihaz çiftlerini durumu ve yapılandırmaları eşitlemek için kullanın][lnk-devguide-device-twins]
* [Bir cihazda doğrudan bir yöntem çağırma][lnk-devguide-directmethods]
* [Birden çok aygıta işleri zamanla][lnk-devguide-jobs]

Bu makalede açıklanan kavramları bazıları denemek istiyorsanız, aşağıdaki IOT hub'ı öğreticileri bakın:

* [Azure IOT Hub ile çalışmaya başlama][lnk-getstarted-tutorial]
* [IOT Hub ile bulut-cihaz iletilerini göndermek nasıl][lnk-c2d-tutorial]
* [IOT Hub cihaz-bulut iletileri işleme][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth._iot_hub_service_sas_token
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-IoT-extension-CLI-2.0]: https://github.com/Azure/azure-iot-cli-extension

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
