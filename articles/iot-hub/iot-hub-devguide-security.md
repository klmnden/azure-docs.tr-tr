---
title: Azure IOT Hub güvenlik anlama | Microsoft Docs
description: Geliştirici Kılavuzu - cihaz uygulamalarını hem de arka uç uygulamaları için IOT hub'a erişimi denetleme. Güvenlik belirteçleri ve X.509 sertifikaları için destek hakkında bilgi içerir.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 227723ecea1401247f0df87bccfe058fb2273647
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39145358"
---
# <a name="control-access-to-iot-hub"></a>IoT Hub’a erişimi denetleme

Bu makalede, IOT hub'ınıza güvenliğini sağlamak için seçenekleri açıklar. IOT Hub kullanan *izinleri* her IOT hub uç noktasına erişim vermek için. İzinleri işlevselliğine bağlı bir IOT hub'a erişimi sınırlayabilir.

Bu makalede sunar:

* Farklı izinler, IOT hub'ınıza erişmek için bir cihaz veya arka uç uygulamasının verebilirsiniz.
* Kimlik doğrulama işlemi ve belirteçlere izinleri doğrulamak için kullanır.
* Erişimi belirli kaynaklara sınırlamak için kimlik bilgilerini nasıl.
* X.509 sertifikaları IOT hub'ı destekler.
* Var olan cihaz kimlik kayıt defterleri veya kimlik doğrulama şeması kullanma özel cihaz kimlik doğrulama mekanizmaları.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IOT Hub uç noktaları erişmek için uygun izinlere sahip olmalıdır. Örneğin, bir cihaz IOT Hub'ına gönderir her bir iletiyle birlikte güvenlik kimlik bilgilerini içeren bir belirteç içermelidir.

## <a name="access-control-and-permissions"></a>Erişim denetimi ve izinleri

Size verebilir [izinleri](#iot-hub-permissions) aşağıdaki yollarla:

* **IOT hub'ı düzeyinde paylaşılan erişim ilkeleri**. Paylaşılan erişim ilkeleri, herhangi bir birleşimini vermek [izinleri](#iot-hub-permissions). İlkeleri tanımlayabilirsiniz [Azure portalında][lnk-management-portal], kullanarak program aracılığıyla [IOT hub'ı kaynak REST API'leri][lnk-resource-provider-apis], veya kullanma[az IOT hub'ı İlkesi](https://docs.microsoft.com/cli/azure/iot/hub/policy?view=azure-cli-latest) CLI. Yeni oluşturulan bir IOT hub aşağıdaki varsayılan ilkeler vardır:
  
  | Paylaşılan Erişim İlkesi | İzinler |
  | -------------------- | ----------- |
  | iothubowner | ALL izni |
  | hizmet | **ServiceConnect** izinleri |
  | cihaz | **DeviceConnect** izinleri |
  | RegistryRead | **RegistryRead** izinleri |
  | RegistryReadWrite | **RegistryRead** ve **RegistryWrite** izinleri |

* **Cihaz başına güvenlik kimlik bilgileri**. Her IOT hub'ı içeren bir [kimlik kayıt defteri][lnk-identity-registry]. Bu kimlik kayıt defterinde her cihaz için vermek güvenlik kimlik bilgilerini yapılandırabileceğiniz **DeviceConnect** karşılık gelen cihaz uç noktalarına için kapsamlı izinler.

Örneğin, tipik bir IOT çözüm içinde:

* Cihaz yönetim bileşeni kullanan *registryReadWrite* ilkesi.
* Olay işlemcisi bileşen *hizmet* ilkesi.
* Çalışma zamanı cihaz iş mantığı bileşen *hizmet* ilkesi.
* Tek tek cihazlar IOT hub'ının kimlik kayıt defterinde depolanan kimlik bilgilerini kullanarak bağlanın.

> [!NOTE]
> Bkz: [izinleri](#iot-hub-permissions) ayrıntılı bilgi için.

## <a name="authentication"></a>Kimlik Doğrulaması

Azure IOT hub'ı paylaşılan erişim ilkeleri ve kimlik kayıt defteri güvenlik kimlik bilgilerini karşı bir belirteci doğrulayarak uç noktalarına erişimi verir.

Simetrik anahtarlar gibi güvenlik kimlik bilgilerini asla kablo üzerinden gönderilir.

> [!NOTE]
> Azure IOT hub'ı kaynak sağlayıcısı, tüm sağlayıcıları gibi Azure aboneliğiniz üzerinden sağlanır [Azure Resource Manager][lnk-azure-resource-manager].

Oluşturun ve güvenlik belirteçleri kullanma hakkında daha fazla bilgi için bkz. [IOT Hub güvenlik belirteçleri][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokol özellikleri

MQTT, AMQP ve HTTPS gibi desteklenen her protokolü, farklı şekillerde belirteçleri taşır.

MQTT kullanırken CONNECT paket ClientID olarak DeviceID sahip `{iothubhostname}/{deviceId}` kullanıcı adı alanı ve parola alanına bir SAS belirteci. `{iothubhostname}` IOT hub'ı (örneğin, contoso.azure-devices.net) tam CName olmalıdır.

Kullanırken [AMQP][lnk-amqp], IOT hub'ın desteklediği [SASL DÜZ] [ lnk-sasl-plain] ve [AMQP talep tabanlı-güvenlikli] [ lnk-cbs].

AMQP talep tabanlı-güvenlikli kullanırsanız, bu belirteçleri iletme nasıl standart belirtir.

SASL DÜZ için **kullanıcıadı** olabilir:

* `{policyName}@sas.root.{iothubName}` IOT hub'ı düzeyinde belirteçleri kullanıyorsanız.
* `{deviceId}@sas.{iothubname}` kapsamlı cihaz belirteçleri kullanıyorsanız.

Her iki durumda da, parola alanına belirteci açıklandığı içeren [IOT Hub güvenlik belirteçleri][lnk-sas-tokens].

Geçerli bir belirteç içine dahil ederek HTTPS kimlik doğrulaması uygulayan **yetkilendirme** isteği üstbilgisi.

#### <a name="example"></a>Örnek

Kullanıcı adı (DeviceID küçük harfe duyarlı): `iothubname.azure-devices.net/DeviceId`

Parola (bir SAS belirteci ile oluşturabileceğiniz [cihaz Gezgini] [ lnk-device-explorer] CLI uzantısı Komut Aracı [az IOT hub'ı oluşturma-sas-token](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-generate-sas-token), veya [Azure IOT Visual Studio Code için Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)):

`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> [Azure IOT SDK'ları] [ lnk-sdks] belirteçleri hizmetine olduğunda otomatik olarak oluşturur. Bazı durumlarda, Azure IOT SDK'ları, tüm protokoller veya tüm kimlik doğrulama yöntemleri desteklemez.

### <a name="special-considerations-for-sasl-plain"></a>SASL DÜZ ilgili özel konular

SASL DÜZ AMQP ile kullanırken, bir IOT hub'ına bağlanan bir istemci her TCP bağlantısı için tek bir belirteç kullanabilirsiniz. Belirtecin süresi dolduğunda, TCP bağlantısı hizmet bağlantısını keser ve bir yeniden tetikler. Bu davranış, sorunlu değil ancak farklı bir arka uç uygulaması için bir cihaz uygulaması için aşağıdaki nedenlerden dolayı zarar:

* Ağ geçidi genellikle çok sayıda cihaz adına bağlanın. SASL DÜZ kullanırken, bunlar bir IOT hub'a bağlanan her cihaz için ayrı bir TCP bağlantı oluşturmanız gerekir. Bu senaryoda önemli ölçüde gücü ve ağ kaynakları tüketiminin artırır ve her bir cihaz bağlantı gecikme süresini artırır.
* Kaynak kısıtlı cihazları artan her belirteç süre dolduktan sonra yeniden bağlanmayı kaynaklarının kullanımını olumsuz etkilenir.

## <a name="scope-iot-hub-level-credentials"></a>IOT hub'ı düzeyinde kimlik kapsamı

Kısıtlanmış bir kaynak URI'si ile belirteçleri oluşturarak IOT hub'ı düzeyinde güvenlik ilkelerinin kapsamını belirleyebilirsiniz. Örneğin, bir CİHAZDAN CİHAZDAN buluta iletileri göndermeye uç noktadır **/devices/ {DeviceID} / iletiler/olaylar**. Bir IOT hub'ı düzeyinde paylaşılan erişim ilkesi ile de kullanabileceğiniz **DeviceConnect** , resourceURI olan bir belirteç imzalamak için izinleri **/devices/ {DeviceID}**. Bu yaklaşım, yalnızca cihaz adına bir ileti göndermek için kullanılabilen bir belirteç oluşturur **DeviceID**.

Bu mekanizma benzer [Event Hubs Yayımcı ilkesi][lnk-event-hubs-publisher-policy]ve özel kimlik doğrulama yöntemleri sağlar.

## <a name="security-tokens"></a>Güvenlik belirteçleri

IOT Hub, cihaz ve hizmet anahtarları kablo göndermekten kaçınmanız kimliğini doğrulamak için güvenlik belirteçleri kullanır. Ayrıca, güvenlik belirteçleri geçerlilik tarihi ve kapsam bakımından sınırlıdır. [Azure IOT SDK'ları] [ lnk-sdks] otomatik olarak özel bir yapılandırma gerektirmeden belirteçleri oluşturun. Bazı senaryolar oluşturur ve güvenlik belirteçlerini doğrudan kullanmanızı gerektirir. Bu tür senaryolar şunlardır:

* MQTT, AMQP veya HTTPS yüzey doğrudan kullanın.
* Belirteç Hizmeti düzeninin açıklandığı şekilde uygulaması [özel cihaz kimlik doğrulamasını][lnk-custom-auth].

IOT Hub ayrıca kullanarak IOT Hub ile kimlik doğrulaması cihazları sağlar [X.509 sertifikaları][lnk-x509].

### <a name="security-token-structure"></a>Güvenlik belirteci yapısı

Zaman sınırlı erişim cihazlara ve IOT hub'ında belirli işlevleri hizmet vermek için güvenlik belirteçleri kullanın. IOT Hub'ına bağlanmak için yetkilendirme almak için cihazlar ve Hizmetler bir paylaşılan erişim veya simetrik anahtar ile imzalanmış güvenlik belirteçleri göndermeniz gerekir. Bu anahtarlar, kimlik kayıt defterinde bir cihaz kimliği ile depolanır.

Paylaşılan Erişim İlkesi izinleri ile ilişkili tüm işlevlere bir paylaşılan erişim anahtarı verir erişimi olan bir belirteci imzalayan. Bir belirteci imzalayan cihaz kimliğin simetrik anahtar yalnızca verir ile **DeviceConnect** ilişkili cihaz kimliği için izni.

Güvenlik belirteci biçimi aşağıdaki gibidir:

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Beklenen değerler şunlardır:

| Değer | Açıklama |
| --- | --- |
| {imzası} |Bir formun SHA256 HMAC imzası dizesi: `{URL-encoded-resourceURI} + "\n" + expiry`. **Önemli**: anahtar base64 kodlaması çözülmüş ve HMAC SHA256 hesaplama gerçekleştirmek için anahtar olarak kullanılan. |
| {resourceURI} |IOT hub'ı (hiçbir Protokolü) ana bilgisayar adı ile başlayarak, bu belirteç ile erişilen uç noktalar (segmente) öneki URI. Örneğin, `myHub.azure-devices.net/devices/device1` |
| {expiry} |UTF8 dizeleri için dönem 00:00:00 UTC 1 Ocak 1970'ten beri geçen saniye sayısı. |
| {URL olarak kodlanmış ResourceURI} |Küçük kaynak URI'si için URL kodlaması düşük durumda |
| {policyName} |Bu belirteç başvurduğu paylaşılan erişim ilkesi adı. Çalıştırıyorsa, cihaz kayıt defteri kimlik belirteci ifade eder. |

**Önek dekontunda**: URI öneki segmente ve karakter tarafından hesaplanır. Örneğin `/a/b` için bir önek `/a/b/c` ancak `/a/bc`.

Aşağıdaki Node.js kod parçacığında çağrılan bir işlev gösterir **generateSasToken** , girişleri belirteçten hesaplar `resourceUri, signingKey, policyName, expiresInMins`. Farklı girdiler için farklı bir belirteç kullanım durumlarını nasıl sonraki bölümlerde ayrıntılı olarak açıklanmaktadır.

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
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

Bir güvenlik belirteci oluşturmak için C# dilinde bir işlevdir:

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
> IOT hub'ı makinelerde belirteç süre geçerliliği doğrulanır olduğundan, belirteci oluşturan makinenin saatindeki kayması en az olmalıdır.

### <a name="use-sas-tokens-in-a-device-app"></a>Bir cihaz uygulama SAS belirteçlerini kullanma

Almanın iki yolu vardır **DeviceConnect** güvenlik belirteçleri ile IOT Hub ile izinleri: kullanan bir [simetrik cihaz kimlik kayıt defteri anahtarından](#use-a-symmetric-key-in-the-identity-registry), veya bir [paylaşılan erişim anahtarı](#use-a-shared-access-policy).

Tasarım ile ön uç üzerinde cihazlardan erişilebilen tüm işlevselliği kullanıma sunulduğunu unutmayın `/devices/{deviceId}`.

> [!IMPORTANT]
> IOT hub'ı belirli bir cihaz kimlik doğrulaması tek yolu cihazı kimlik simetrik anahtar kullanıyor. Cihaz işlevine erişmek için bir paylaşılan erişim ilkesi kullanıldığında durumlarda güvenilen bir alt bileşen olarak güvenlik belirteci verilirken bileşen çözüm dikkate almanız gerekir.

(Bağımsız olarak Protokolü) cihaz'e yönelik uç noktalar şunlardır:

| Uç Nokta | İşlev |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |CİHAZDAN buluta iletileri gönderir. |
| `{iot hub host name}/devices/{deviceId}/messages/devicebound` |Bulut-cihaz iletilerini alır. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Simetrik anahtar kimlik kayıt defterinde kullanın

Bir cihaz kimliğin simetrik anahtar birleştirilerek bir belirteç oluşturmak için kullanırken (`skn`) öğesi belirtecin atlanmış.

Örneğin, aşağıdaki parametreleri oluşturulan tüm cihaz işlevine erişmek için bir belirteç olması gerekir:

* Kaynak URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* imzalama anahtarı: herhangi bir simetrik anahtar için `{device id}` kimliği
* İlke adı yok
* herhangi bir sona erme saati.

Önceki bir Node.js işlevini kullanarak bir örnek şöyle olacaktır:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

Cihaz 1 için tüm işlevlere erişim verir, sonuç şu şekilde olur:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> Bir SAS belirteci oluşturmak mümkündür [cihaz Gezgini] [ lnk-device-explorer] CLI uzantısı Komut Aracı [az IOT hub'ı oluşturma-sas-token](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-generate-sas-token), veya [Azure IOT Visual Studio Code için Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).

### <a name="use-a-shared-access-policy"></a>Paylaşılan Erişim İlkesi kullanın

Paylaşılan Erişim İlkesi tarafından bir belirteç oluşturduğunuzda, `skn` ilke adı alanı. Bu ilke vermelisiniz **DeviceConnect** izni.

Paylaşılan erişim ilkeleri cihaz işlevine erişmek için kullanılan iki ana senaryolar şunlardır:

* [protokol ağ geçitleri bulut][lnk-endpoints],
* [simge Hizmetleri] [ lnk-custom-auth] özel kimlik doğrulama düzenleri uygulamak için kullanılır.

Paylaşılan erişim ilkesi, potansiyel herhangi bir CİHAZDAN bağlanmak için erişim izni verebilirsiniz olduğundan, doğru kaynak URI'si güvenlik belirteçleri oluştururken kullanmak önemlidir. Bu ayar, kaynak URI'si kullanarak belirli bir cihaz belirteci kapsam için belirteci Hizmetleri için özellikle önemlidir. Bu noktaya zaten tüm cihazlar için trafiği eşlemlerse olarak protokol ağ geçitleri için daha az uygundur.

Örneğin, önceden oluşturulmuş kullanarak belirteç hizmeti paylaşılan erişim ilkesi adlı **cihaz** aşağıdaki parametrelerle bir belirteç oluşturur:

* Kaynak URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* imzalama anahtarı: anahtarlarından `device` İlkesi
* İlke adı: `device`,
* herhangi bir sona erme saati.

Önceki bir Node.js işlevini kullanarak bir örnek şöyle olacaktır:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Cihaz 1 için tüm işlevlere erişim verir, sonuç şu şekilde olur:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

Protokol ağ geçidi, tüm cihazlar yalnızca Kaynak URI ayarı için aynı belirteci kullanabilirsiniz için `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Hizmet bileşenleri güvenlik belirteçleri kullanma

Hizmet bileşenleri, yalnızca güvenlik belirteçleri kullanarak daha önce açıklandığı gibi uygun izinleri verip paylaşılan erişim ilkeleri oluşturabilirsiniz.

Bitiş noktası kullanıma sunulan hizmet işlevleri şunlardır:

| Uç Nokta | İşlev |
| --- | --- |
| `{iot hub host name}/devices` |Oluşturma, güncelleştirme, alma ve cihaz kimliklerini silme. |
| `{iot hub host name}/messages/events` |CİHAZDAN buluta iletiler alır. |
| `{iot hub host name}/servicebound/feedback` |Bulut-cihaz iletilerini için geri bildirim alın. |
| `{iot hub host name}/devicebound` |Bulut-cihaz iletilerini gönderin. |

Örnek olarak, paylaşılan erişim ilkesi adlı önceden oluşturulmuş kullanarak bir hizmet oluşturma **registryRead** aşağıdaki parametrelerle bir belirteç oluşturur:

* Kaynak URI: `{IoT hub name}.azure-devices.net/devices`,
* imzalama anahtarı: anahtarlarından `registryRead` İlkesi
* İlke adı: `registryRead`,
* herhangi bir sona erme saati.

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Tüm cihaz kimliklerini okumak üzere erişim vermek, sonuç şu şekilde olur:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a>Desteklenen X.509 sertifikaları

Herhangi bir X.509 sertifikası, sertifika parmak izi veya bir sertifika yetkilisi (CA), Azure IOT Hub'ına yükleyerek bir cihaz IOT Hub ile kimlik doğrulaması için kullanabilirsiniz. Yalnızca sertifika parmak izleri kullanılarak kimlik doğrulaması, sunulan parmak izi yapılandırılmış parmak iziyle eşleştiğini doğrular. Sertifika yetkilisi kullanılarak kimlik doğrulaması sertifika zincirini doğrular. 

Desteklenen sertifikaları dahil et:

* **Mevcut bir X.509 sertifikası**. Bir cihaz zaten kendisiyle ilişkilendirilmiş bir X.509 sertifikası olabilir. Cihaz, bu sertifika, IOT Hub ile kimlik doğrulaması için kullanabilirsiniz. Parmak izi veya CA kimlik doğrulaması ile çalışır. 
* **CA imzalı X.509 sertifikası**. Bir cihazı tanımlamak ve IOT Hub ile kimlik doğrulaması yapmak için oluşturulan ve bir sertifika yetkilisi (CA) tarafından imzalanmış bir X.509 sertifikası kullanabilirsiniz. Parmak izi veya CA kimlik doğrulaması ile çalışır.
* **Bir şirket içinde oluşturulan ve otomatik olarak imzalanan sertifika X-509**. Cihaz üreticisi veya şirket içi dağıtıcısı, bu sertifikaları oluşturmak ve karşılık gelen özel anahtar (ve sertifika) cihazda saklayın. Gibi araçları kullanın [OpenSSL] [ lnk-openssl] ve [Windows SelfSignedCertificate] [ lnk-selfsigned] bu amaç için yardımcı program. Parmak izi kimlik yalnızca çalışır. 

Bir cihaz ya da bir X.509 sertifikası veya bir güvenlik belirteci kimlik doğrulaması, ancak ikisini birden kullanabilir.

Sertifika yetkilisi kullanılarak kimlik doğrulaması hakkında daha fazla bilgi için bkz: [cihaz X.509 CA sertifikalarını kullanarak kimlik doğrulaması](iot-hub-x509ca-overview.md).

### <a name="register-an-x509-certificate-for-a-device"></a>Bir cihaz için bir X.509 sertifikası kaydetme

[C# için Azure IOT hizmeti SDK'sı] [ lnk-service-sdk] (sürüm 1.0.8+) kimlik doğrulaması için bir X.509 sertifikası kullanan bir cihazı kaydetme destekler. Cihaz içeri/dışarı aktarma gibi diğer API'ler, X.509 sertifikalarını da destekler.

CLI uzantısını komutunu da kullanabilirsiniz [az IOT hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) X.509 sertifikaları cihazlar için yapılandırma.

### <a name="c-support"></a>C\# desteği

**RegistryManager** sınıfı, bir cihaz kaydetmek için programlı bir yol sağlar. Özellikle, **AddDeviceAsync** ve **UpdateDeviceAsync** kaydolmanızı ve IOT Hub kimlik kayıt defterinde bir cihaz güncelleştirme yöntemleri sağlar. Bu iki yöntem ele bir **cihaz** örneği girdi olarak. **Cihaz** sınıfı içeren bir **kimlik doğrulaması** birincil ve ikincil X.509 sertifika parmak izleri belirtmenize olanak tanıyan özellik. Parmak izi (DER ikili kodlama kullanılarak depolanır) X.509 Sertifika SHA256 karmasını temsil eder. Birincil parmak izi veya ikincil bir parmak izi veya her ikisini de belirtme seçeneğiniz vardır. Birincil ve ikincil parmak izleri sertifika geçiş senaryoları işlemek için desteklenir.

İşte bir örnek C\# bir X.509 sertifikası parmak izi'ni kullanarak bir cihazı kaydetmek için kod parçacığı:

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "B4172AB44C28F3B9E117648C6F7294978A00CDCBA34A46A1B8588B3F7D82C4F1"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Çalışma zamanı işlemleri sırasında bir X.509 sertifikası kullanma

[.NET için Azure IOT cihaz SDK'sını] [ lnk-client-sdk] (sürüm 1.0.11+) X.509 sertifikaları kullanılmasını destekler.

### <a name="c-support"></a>C\# desteği

Sınıf **DeviceAuthenticationWithX509Certificate** oluşturmayı destekler **DeviceClient** bir X.509 sertifikası kullanarak örnekler. X.509 sertifikasının özel anahtarı içeren PFX (PKCS #12 olarak da bilinir) biçiminde olmalıdır.

Örnek kod parçacığı aşağıda verilmiştir:

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-and-module-authentication"></a>Özel cihaz ve modülü kimlik doğrulama

IOT hub'ı kullanabileceğiniz [kimlik kayıt defteri] [ lnk-identity-registry] cihaz/modül başına güvenlik kimlik bilgilerini yapılandırın ve erişim denetimi kullanarak [belirteçleri] [ lnk-sas-tokens]. Bir IOT çözümündeki bir özel kimlik kayıt defteri ve/veya kimlik doğrulama düzeni zaten varsa, oluşturmayı göz önünde bulundurun bir *belirteç hizmeti* bu altyapı, IOT Hub ile tümleştirmek için. Bu şekilde, çözümünüzdeki diğer IOT özelliklerini kullanabilirsiniz.

Belirteç Hizmeti bir özel bulut hizmetidir. IOT hub'ı kullanan *paylaşılan erişim ilkesi* ile **DeviceConnect** veya **ModuleConnect** oluşturma izni *cihaz kapsamlı* veya *modülü kapsamlı* belirteçleri. Bu belirteçler, cihaz ve IOT hub'ınıza bağlanmak için modül etkinleştirin.

![Belirteç Hizmeti deseninin adımları][img-tokenservice]

Belirteç Hizmeti deseninin ana adımlar şunlardır:

1. İle IOT hub'ı paylaşılan erişim ilkesi oluşturma **DeviceConnect** veya **ModuleConnect** IOT hub'ınız için izinleri. Bu ilkede oluşturabilirsiniz [Azure portalında] [ lnk-management-portal] veya programlama yoluyla. Belirteç Hizmeti bu ilkenin oluşturduğu Belirteçleri imzalamak için kullanır.
1. IOT hub'ınıza erişmek cihaz/modülü ihtiyacı olduğunda, belirteci Hizmeti'nden imzalı bir belirteç ister. Cihaz belirteci hizmetin belirteci oluşturmak için kullandığı cihaz/modül kimliği belirlemek için özel kimlik kayıt defteri/kimlik doğrulama düzeni ile kimlik doğrulaması yapabilir.
1. Belirteç Hizmeti bir belirteç döndürür. Belirteci kullanılarak oluşturulan `/devices/{deviceId}` veya `/devices/{deviceId}/module/{moduleId}` olarak `resourceURI`, ile `deviceId` olarak cihaz kimlik doğrulaması gerçekleştirilen veya `moduleId` kimlik doğrulaması gerçekleştirilen modül olarak. Belirteç Hizmeti, belirteci oluşturmak için paylaşılan erişim ilkesi kullanır.
1. Cihaz/modül Bu belirteci doğrudan IOT hub ile kullanır.

> [!NOTE]
> .NET sınıf kullanabileceğiniz [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] veya Java sınıfı [IotHubServiceSasToken] [ lnk-java-sas] bir belirteç oluşturmak için Belirteç Hizmeti.

Belirteç Hizmeti, belirteci süre sonu istediğiniz gibi ayarlayabilirsiniz. Belirtecin süresi dolduğunda, IOT hub cihaz/modülü bağlantı sunucularından. Ardından, cihaz/modül belirteci Hizmeti'nden yeni bir belirteç isteği göndermelidir. Kısa bir süre sonu zamanı cihaz/modülü hem belirteci hizmet üzerindeki yükü artırır.

Bir cihaz/hub'ınıza bağlanmak için modülü, hala onu IOT Hub kimlik kayıt defterinde eklemeniz gerekir — bir belirteç ve bir anahtar bağlanmak için kullandığı olsa bile. Bu nedenle, etkinleştirme veya cihaz/modül kimliklerini devre dışı bırakarak başına-cihaz/modül başına erişim denetimi kullanmaya devam edebilirsiniz [kimlik kayıt defteri][lnk-identity-registry]. Bu yaklaşım, uzun süre sonu süreleri ile belirteçleri kullanarak riskleri azaltır.

### <a name="comparison-with-a-custom-gateway"></a>Özel bir ağ geçidi ile karşılaştırma

Belirteç Hizmeti, IOT hub'ı ile bir özel kimlik kayıt defteri/kimlik doğrulama düzeni uygulamak için önerilen yöntem modelidir. Çoğu çözüm trafiğini işlemek IOT hub'ı devam ettiğinden bu düzen önerilir. Ancak, özel kimlik doğrulama şeması bu nedenle protokolü ile birbirine, gerektirebilecek bir *özel ağ geçidi* tüm trafiği işlemek için. Böyle bir senaryo örneği kullanarak[Aktarım Katmanı Güvenliği (TLS) ve önceden paylaşılan anahtarları (PSKs)][lnk-tls-psk]. Daha fazla bilgi için [protokol ağ geçidi] [ lnk-protocols] makalesi.

## <a name="reference-topics"></a>Başvuru konuları:

Aşağıdaki başvuru konuları, IOT hub'ınıza erişimi denetleme hakkında daha fazla bilgi sağlar.

## <a name="iot-hub-permissions"></a>IOT hub'ı izinleri

Aşağıdaki tabloda, IOT hub'ınıza erişimi denetlemek için kullanabileceğiniz izinleri listeler.

| İzin | Notlar |
| --- | --- |
| **RegistryRead** |Okuma kimlik kayıt defterine erişim verir. Daha fazla bilgi için [kimlik kayıt defteri][lnk-identity-registry]. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **RegistryReadWrite** |Okuma ve yazma erişimi için kimlik kayıt defteri. Daha fazla bilgi için [kimlik kayıt defteri][lnk-identity-registry]. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **ServiceConnect** |Bulut hizmeti kullanıma yönelik iletişim ve uç noktalarınızı izleyerek erişimi verir. <br/>CİHAZDAN buluta iletileri almak, bulut-cihaz iletilerini göndermek ve karşılık gelen teslim alındı bildirimleri almak için izin verir. <br/>Karşıya yükleme dosyası için teslim bildirimleri almak için izin verir. <br/>Etiketler ve istenen özellikleri güncelleştirme, bildirilen özellikleri almak ve sorguları çalıştırmak için erişim ikizlerini izin verir. <br/>Bu izin, arka uç bulut Hizmetleri tarafından kullanılır. |
| **DeviceConnect** |Cihaz'e yönelik uç noktalarına erişimi verir. <br/>CİHAZDAN buluta iletiler gönderir ve bulut-cihaz iletilerini izni verir. <br/>Bir CİHAZDAN karşıya dosya yükleme gerçekleştirme izni verir. <br/>Bildirilen özellikler cihaz çiftinin istenen özellik bildirimleri almak ve cihaz ikizi güncelleştirmek için izin verir. <br/>Dosya gerçekleştirilecek izin verir yükler. <br/>Bu izin, cihazlar tarafından kullanılır. |

## <a name="additional-reference-material"></a>Ek başvuru malzemesi

IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] her IOT hub'ı ortaya koyan çalışma zamanı ve yönetim işlemleri için çeşitli uç noktaları açıklar.
* [Azaltma ve kotalar] [ lnk-quotas] kotaları açıklar ve IOT Hub hizmetine geçerli davranışlara azaltma.
* [Azure IOT cihaz ve hizmet SDK'ları] [ lnk-sdks] çeşitli dil IOT hub'ı ile etkileşim kuran hem cihaz hem de hizmet uygulamaları geliştirirken kullanabileceğiniz SDK'ları listeler.
* [IOT Hub sorgu dili] [ lnk-query] IOT Hub'ından, cihaz ikizleri ve işler hakkında bilgi almak için kullanabileceğiniz bir sorgu dili açıklar.
* [IOT hub'ı MQTT desteği] [ lnk-devguide-mqtt] ve MQTT protokolünü için IOT hub'ı desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'a erişimi denetleme öğrendiğinize göre aşağıdaki IOT Hub Geliştirici Kılavuzu konuları ilginizi çekebilir:

* [Durum ve yapılandırmaları eşitlemek için cihaz ikizlerini kullanma][lnk-devguide-device-twins]
* [Bir cihazda doğrudan yöntem çağırma][lnk-devguide-directmethods]
* [Birden fazla cihazda işleri zamanlama][lnk-devguide-jobs]

Bu makalede açıklanan kavramları bazıları denemek istiyorsanız, aşağıdaki öğreticilerde IOT Hub bakın:

* [Azure IOT Hub ile çalışmaya başlama][lnk-getstarted-tutorial]
* [IOT Hub ile bulut buluttan cihaza iletiler gönderme][lnk-c2d-tutorial]
* [IOT Hub CİHAZDAN buluta iletiler nasıl işlenir?][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate

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
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-and-module-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-getstarted-tutorial]: quickstart-send-telemetry-node.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: tutorial-routing.md
