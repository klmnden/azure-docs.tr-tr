---
title: Docker kapsayıcı ayarları için Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS kapsayıcı çalışma zamanı ortamı kullanılarak yapılandırılan `docker run` komut bağımsız değişkenleri. LUIS birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: diberry
ms.openlocfilehash: 020c623f881dd806cbc42b72596a2cc87e29045b
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52965062"
---
# <a name="configure-containers"></a>Kapsayıcıları yapılandırma

Language Understanding (LUIS) kapsayıcı çalışma zamanı ortamı kullanılarak yapılandırılan `docker run` komut bağımsız değişkenleri. LUIS birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır. Birkaç [örnekler](#example-docker-run-commands) komutu kullanılabilir. Giriş kapsayıcısı özgü ayarlar şunlardır [bağlama ayarları](#mount-settings) ve fatura ayarlar. 

Kapsayıcı ayarları [hiyerarşik](#hierarchical-settings) ve ayarlanabilir [ortam değişkenlerini](#environment-variable-settings) ya da docker [komut satırı bağımsız değişkenleri](#command-line-argument-settings).

## <a name="configuration-settings"></a>Yapılandırma ayarları

Bu kapsayıcı, aşağıdaki yapılandırma ayarları vardır:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-setting)|Fatura bilgileri izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği.|
|Evet|[Faturalandırma](#billing-setting)|URI uç noktasını belirtir, _Language Understanding_ azure'da kaynak.|
|Evet|[EULA'sı](#eula-setting)| Kapsayıcı lisansını kabul ettiğiniz gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük yazma ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.|
|Hayır|[Günlüğe kaydetme](#logging-settings)|ASP.NET Core günlüğü kapsayıcınız için destek sağlar. |
|Evet|[Bağlar](#mount-settings)|Okuma ve yazma verilerinden [ana bilgisayar](luis-container-howto.md#the-host-computer) kapsayıcı ve kapsayıcıya geri ana bilgisayar.|

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-setting), [ `Billing` ](#billing-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır ve bunları; Aksi takdirde, tüm üç için geçerli değerler sağlamanız gerekir kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>ApiKey ayarı

`ApiKey` Ayar kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer için geçerli bir anahtar olmalıdır _Language Understanding_ için belirtilen kaynak [ `Billing` ](#billing-setting) yapılandırma ayarı.

Bu ayar, iki yerde bulunabilir:

* Azure portalı: **Language Understanding'ın** kaynak yönetimi altında **anahtarları**
* LUIS portalı: **anahtarları ve uç nokta ayarları** sayfası. 

Başlangıç veya geliştirme tuşuna kullanmayın. 

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

`ApplicationInsights` Ayarı eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği. Application Insights, kapsayıcının kapsamlı izleme sağlar. Kapsayıcınızı kullanılabilirliğini, performansını ve kullanımını kolayca izleyebilir. Ayrıca bir kolayca belirleyin ve kapsayıcınızda hatalarını tanılayın.

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `ApplicationInsights` bölümü.

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Hayır| `InstrumentationKey` | Dize | İzleme anahtarı Application Insights örneğinin hangi telemetri veri kapsayıcısı için gönderilir. Daha fazla bilgi için [ASP.NET Core için Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Örnek:<br>`InstrumentationKey=123456789`|


## <a name="billing-setting"></a>Faturalandırma ayarı

`Billing` Ayar uç noktası URI'si belirtir, _Language Understanding_ azure'da kaynak kapsayıcısı için fatura bilgileri ölçmek için kullanılır. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değeri geçerli bir uç noktası URI'si olmalıdır için bir _Language Understanding_ azure'da kaynak.

Bu ayar, iki yerde bulunabilir:

* Azure portalı: **Language Understanding'ın** etiketli genel bakış `Endpoint`
* LUIS portalı: **anahtarları ve uç nokta ayarları** URI uç noktasının bir parçası olarak bir sayfa.

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

## <a name="eula-setting"></a>EULA'yı ayarlama

`Eula` Ayarı, kapsayıcı lisansını kabul ettiğiniz belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer ayarlanmalıdır `accept`.

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Eula` | Dize | Lisans kabulü<br><br>Örnek:<br>`Eula=accept` |

Bilişsel hizmetler kapsayıcılar, Azure kullanımını düzenleyen sözleşmenize altında lisanslanmıştır. Azure kullanımınızı var olan bir anlaşma değilse, Azure'un kullanımını düzenleyen sözleşmenize Microsoft çevrimiçi abonelik (çevrimiçi hizmet koşulları kullanımız) sözleşmesi olduğunu kabul etmiş olursunuz. Önizlemeler için de ek kullanım koşulları için Microsoft Azure önizlemeleri için kabul etmiş olursunuz. Kapsayıcı kullanarak bu koşulları kabul etmiş olursunuz.

## <a name="fluentd-settings"></a>Fluentd ayarları

Bir açık kaynak veri toplayıcı birleşik günlük kaydı için Fluentd olur. `Fluentd` Ayarlarını yönetmek için kapsayıcının bağlantı bir [Fluentd](https://www.fluentd.org) sunucusu. LUIS kapsayıcısı, kapsayıcının günlüklerini yazma izni verir Fluentd oturum açma sağlayıcısı içerir ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `Fluentd` bölümü.

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Host` | Dize | Fluentd sunucusunun DNS ana bilgisayar adını veya IP adresi. |
| `Port` | Tamsayı | Fluentd sunucusunun bağlantı noktası.<br/> 24224 varsayılan değerdir. |
| `HeartbeatMs` | Tamsayı | Milisaniye cinsinden sinyal aralığı. Bu aralığı süresi dolmadan önce olay trafik gönderildi, bir sinyal Fluentd sunucuya gönderilir. 60000 milisaniye (1 dakika) varsayılan değerdir. |
| `SendBufferSize` | Tamsayı | Gönderme işlemleri için ayrılan bayt cinsinden ağ arabellek alanı. 32768 bayt (32 kilobayt) varsayılan değerdir. |
| `TlsConnectionEstablishmentTimeoutMs` | Tamsayı | Zaman aşımı, Fluentd sunucusuyla bir SSL/TLS bağlantı kurmak için milisaniye cinsinden. 10000 milisaniye (10 saniye) varsayılan değerdir.<br/> Varsa `UseTLS` yanlış olarak bu değeri yok sayıldı ayarlanmış. |
| `UseTLS` | Boole | Kapsayıcı Fluentd sunucusu ile iletişim kurmak için SSL/TLS kullanıp kullanmayacağını belirtir. Varsayılan değer false'tur. |

## <a name="logging-settings"></a>Günlük ayarları

`Logging` Ayarlarını yönetme kapsayıcınız için ASP.NET Core oturum açma desteği. Bir ASP.NET Core uygulaması için kullandığınız kapsayıcınız için aynı yapılandırma ayarları ve değerleri kullanabilirsiniz. 

Aşağıdaki günlük kaydı sağlayıcıları LUIS kapsayıcı tarafından desteklenir:

|Sağlayıcı|Amaç|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|ASP.NET Core `Console` oturum açma sağlayıcısı. Tüm ASP.NET Core yapılandırma ayarlarını ve bu oturum açma sağlayıcısı için varsayılan değerleri desteklenir.|
|[Hata ayıklama](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|ASP.NET Core `Debug` oturum açma sağlayıcısı. Tüm ASP.NET Core yapılandırma ayarlarını ve bu oturum açma sağlayıcısı için varsayılan değerleri desteklenir.|
|[Disk](#disk-logging)|JSON oturum açma sağlayıcısı. Bu oturum açma sağlayıcısı için çıktı bağlama günlük verilerini yazar.|

### <a name="disk-logging"></a>Disk günlüğe kaydetme
  
`Disk` Oturum açma sağlayıcısı, aşağıdaki yapılandırma ayarları destekler:  

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Format` | Dize | Günlük dosyaları için çıkış biçimi.<br/> **Not:** bu değer ayarlanmalıdır `json` günlük sağlayıcısını etkinleştirmek için. Bu değer aynı zamanda bir kapsayıcı örneği oluşturulurken bir çıkış bağlama belirtmeden belirtilirse, bir hata oluşur. |
| `MaxFileSize` | Tamsayı | Megabayt (MB) günlük dosyasının en büyük boyutu. Yeni bir günlük dosyası, geçerli günlük dosyası boyutunu karşıladığından veya bu değeri aşarsa, oturum açma sağlayıcısı tarafından başlatılır. -1 belirtilmezse, günlük dosyasının boyutu, çıkış bağlama yalnızca en büyük dosya boyutuyla sınırlıdır. Varsayılan değer 1'dir. |

ASP.NET Core günlük desteği yapılandırma hakkında daha fazla bilgi için bkz. [ayarları dosya Yapılandırması](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).

## <a name="mount-settings"></a>Bağlama ayarları

Kullanım bağlama okumak ve kapsayıcı gelen ve giden veri yazmak için bağlar. Bir giriş bağlama belirtin veya çıkış bağlama belirterek `--mount` seçeneğini [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutu. 

LUIS kapsayıcı giriş kullanmaz ya da eğitim veya hizmeti verilerini depolamak için çıkış bağlar. 

Konak bağlama konumu söz dizimi konak işletim sistemine göre değişir. Ayrıca, [ana bilgisayar](luis-container-howto.md#the-host-computer)'s bağlama konumu docker hizmet hesabı tarafından kullanılan izinler arasında bir çakışma nedeniyle erişilebilir olmayabilir ve konak yeri izinleri bağlayın. 

Aşağıdaki tabloda, desteklenen ayarları açıklanmaktadır.

|Gerekli| Ad | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|Evet| `Input` | Dize | Giriş bağlama hedefi. Varsayılan değer `/input` şeklindedir. LUIS paket dosyalarının konumunu budur. <br><br>Örnek:<br>`--mount type=bind,src=c:\input,target=/input`|
|Hayır| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer `/output` şeklindedir. Bu günlükler konumdur. Bu, LUIS sorgu ve kapsayıcı günlükleri içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Hiyerarşik ayarları

LUIS kapsayıcısı için ayarları, hiyerarşik ve tüm kapsayıcıları [ana bilgisayar](luis-container-howto.md#the-host-computer) paylaşılan bir hiyerarşi kullanın.

Ayarları belirtmek için aşağıdakilerden birini kullanabilirsiniz:

* [Ortam değişkenleri](#environment-variable-settings)
* [Komut satırı bağımsız değişkenleri](#command-line-argument-settings)

Ortam değişkeni değerlerini, kapsayıcı görüntüsü için varsayılan değerleri sırayla geçersiz komut satırı bağımsız değişkeni değerlerini geçersiz kılar. Bir ortam değişkeni ve bir komut satırı bağımsız değişkeni aynı yapılandırma ayarı için farklı değerler belirtirseniz, ortam değişkeninin değeri örneklenmiş kapsayıcı tarafından kullanılır.

|Öncellik|Konum ayarlama|
|--|--|
|1|Ortam değişkeni| 
|2|Komut Satırı|
|3|Kapsayıcı görüntüsü varsayılan değeri|

### <a name="environment-variable-settings"></a>Ortam değişkeni ayarlarının

Ortam değişkenlerini kullanmanın avantajları şunlardır:

* Birden çok ayarlar yapılandırılabilir.
* Birden çok kapsayıcı aynı ayarları kullanabilirsiniz.

### <a name="command-line-argument-settings"></a>Komut satırı bağımsız değişkeni ayarları

Her kapsayıcı farklı ayarlar kullanabileceğiniz komut satırı bağımsız değişkenleri kullanmanın faydası olur.

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları.  Kapsayıcıyı çalıştıran sonra dek çalıştırmaya devam [Durdur](luis-container-howto.md#stop-the-container) bu.


* **Satır devamlılığı karakteri**: Aşağıdaki bölümlerde docker komutları ters eğik çizgi kullanın `\`, satır devamı karakteri olarak. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin. 
* **Bağımsız değişken sırası**: docker kapsayıcısıyla çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.

Yerine {_argument_name_} kendi değerlerinizle:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
|{ENDPOINT_KEY} | Eğitilen LUIS uygulama uç noktası anahtarı. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | Fatura uç nokta değerini Azure portalının dil anlama genel bakış sayfasında kullanılabilir.|https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](luis-container-howto.md#billing).
> ApiKey değer **anahtar** anahtarları ve uç noktaları sayfasında LUIS portalda ve Azure dil anlama kaynak anahtarlar sayfasında kullanılabilir. 

### <a name="basic-example"></a>Temel örnek

Aşağıdaki örnek, kapsayıcıyı çalıştırmak olası en az sayıda bağımsız değişkenlere sahiptir:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> Yukarıdaki komut dizini aracınızdan `c:` sürücü Windows üzerinde hiçbir izni çakışmalarını önlemek için. Giriş dizini belirli bir dizini kullanmak istiyorsanız, docker vermeniz gerekebilir hizmet izni. Yukarıdaki komut docker ters eğik çizgi kullanır `\`, satır devamı karakteri olarak. Bu temel kaldırın veya değiştirin, [ana bilgisayar](luis-container-howto.md#the-host-computer) işletim sisteminin gereksinimleri. Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.


### <a name="applicationinsights-example"></a>Applicationınsights örneği

Aşağıdaki örnek, kapsayıcıyı çalışırken Application Insights'a telemetri göndermek Applicationınsights bağımsız değişken ayarlar:

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example-with-command-line-arguments"></a>Komut satırı bağımsız değişkenleri ile günlük örnek

Aşağıdaki komut günlüğe kaydetme düzeyini ayarlar `Logging:Console:LogLevel`, günlüğe kaydetme düzeyini yapılandırmak için [ `Information` ](https://msdn.microsoft.com). 

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY} \
Logging:Console:LogLevel=Information
```

### <a name="logging-example-with-environment-variable"></a>Günlük örnek ortam değişkeni

Adlandırılmış bir ortam değişkeni aşağıdaki komutları kullanma `Logging:Console:LogLevel` günlüğe kaydetme düzeyini yapılandırmak için [ `Information` ](https://msdn.microsoft.com). 

```bash
SET Logging:Console:LogLevel=Information
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={APPLICATION_ID} \
```

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [yükleme ve kapsayıcıları çalıştırın](luis-container-howto.md)
* Başvurmak [sık sorulan sorular (SSS)](luis-resources-faq.md) LUIS işlevselliği ile ilgili sorunları gidermek için.