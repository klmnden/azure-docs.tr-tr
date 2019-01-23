---
title: Kapsayıcı - görüntü işleme yapılandırın
titlesuffix: Azure Cognitive Services
description: Görüntü işleme metin tanıma kapsayıcılar için çeşitli ayarları yapılandırın.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 97de65acf724d12afd131ede25713e8f29d30bad
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54477644"
---
# <a name="configure-recognize-text-containers"></a>Metni Tanı kapsayıcılar'ı yapılandırma

Görüntü işleme, ortak bir yapılandırma çerçeve metni tanı kapsayıcıyla sağlar, böylece kolayca yapılandırabilir ve depolama, günlüğe kaydetme ve telemetri ve güvenlik ayarları için kapsayıcılarınızı yönetin.

## <a name="configuration-settings"></a>Yapılandırma ayarları

Görüntü işleme kapsayıcıları yapılandırma ayarlarında hiyerarşik ve tüm kapsayıcıları aşağıdaki üst düzey yapıya dayanarak, paylaşılan bir hiyerarşi:

* [ApiKey](#apikey-configuration-setting)
* [ApplicationInsights](#applicationinsights-configuration-settings)
* [Kimlik doğrulaması](#authentication-configuration-settings)
* [Faturalandırma](#billing-configuration-setting)
* [EULA'sı](#eula-configuration-setting)
* [Fluentd](#fluentd-configuration-settings)
* [HTTP proxy kimlik bilgileri ayarları](#http-proxy-credentials-settings)
* [Günlüğe kaydetme](#logging-configuration-settings)
* [Bağlar](#mounts-configuration-settings)

Kullanabilirsiniz [ortam değişkenlerini](#configuration-settings-as-environment-variables) veya [komut satırı bağımsız değişkenleri](#configuration-settings-as-command-line-arguments) görüntü işleme kapsayıcıların bir kapsayıcı örneği oluşturulurken yapılandırma ayarlarını belirtmek için.

Ortam değişkeni değerlerini, kapsayıcı görüntüsü için varsayılan değerleri sırayla geçersiz komut satırı bağımsız değişkeni değerlerini geçersiz kılar. Diğer bir deyişle, farklı değerler bir ortam değişkeni ve bir komut satırı bağımsız değişkeni aynı yapılandırma ayarı için aşağıdakiler gibi belirtirseniz `Logging:Disk:LogLevel`, ardından bir kapsayıcı örneği, ortam değişkeninin değeri kullanılır tarafından oluşturulmuş kapsayıcı.

### <a name="configuration-settings-as-environment-variables"></a>Ortam değişkenleri olarak yapılandırma ayarları

Kullanabileceğiniz [ASP.NET Core ortam değişkeni sözdizimini](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#environment-variables-configuration-provider) yapılandırma ayarlarını belirtmek için.

Kapsayıcı örneği oluşturulduğunda kapsayıcı kullanıcı ortam değişkenlerini okur. Bir ortam değişkeni varsa, ortam değişkeninin değerini belirtilen bir yapılandırma ayarı için varsayılan değeri geçersiz kılar. Ortam değişkenlerini kullanmanın avantajı, birden çok yapılandırma ayarları, kapsayıcı örneğini oluşturmadan önce ayarlanabilir ve birden çok kapsayıcı otomatik olarak aynı yapılandırma ayarları kümesini kullanabilirsiniz ' dir.

Örneğin, aşağıdaki komutları için konsol günlüğe kaydetme düzeyini yapılandırmak için bir ortam değişkenini kullanmak [LogLevel.Information](https://msdn.microsoft.com), sonra metni tanı kapsayıcı görüntüsünden bir kapsayıcı oluşturur. Ortam değişkeninin değerini, varsayılan yapılandırma ayarını geçersiz kılar.

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0 ApiKey=0123456789
  ```

### <a name="configuration-settings-as-command-line-arguments"></a>Komut satırı bağımsız değişkenleri olarak yapılandırma ayarları

Kullanabileceğiniz [ASP.NET Core komut satırı bağımsız değişkeni sözdizimini](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#arguments) yapılandırma ayarlarını belirtmek için.

İsteğe bağlı yapılandırma ayarlarını belirtebilirsiniz `ARGS` parametresinin [docker run](https://docs.docker.com/engine/reference/commandline/run/) indirilen kapsayıcı görüntüsünden bir kapsayıcı örneği oluşturmak için kullanılan komutu. Her kapsayıcı farklı bir özel yapılandırma ayarları kümesini kullanabileceğiniz komut satırı bağımsız değişkenleri kullanmanın faydası olur.

Örneğin, aşağıdaki komut metni tanı kapsayıcı görüntüsünden bir kapsayıcı oluşturur ve günlük düzeyi için varsayılan yapılandırma ayarı geçersiz kılma LogLevel.Information, konsol yapılandırır.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0 ApiKey=0123456789 Logging:Console:LogLevel=Information
  ```

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

`ApiKey` Yapılandırma ayarı görüntü işleme kaynağının yapılandırma anahtarı kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure üzerinde belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer için belirtilen görüntü işleme kaynak için geçerli yapılandırma anahtar olmalıdır [ `Billing` ](#billing-configuration-setting) yapılandırma ayarı.

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-configuration-setting) yapılandırma ayarlarını birlikte kullanılır ve üçünü de için geçerli değerler sağlamanız gerekir bunları; Aksi takdirde kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](computer-vision-how-to-install-containers.md#billing).

## <a name="applicationinsights-configuration-settings"></a>Applicationınsights yapılandırma ayarları

Yapılandırma ayarlarında `ApplicationInsights` bölümü eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği. Application Insights, kapsayıcınızın kod düzeyine derinlemesine izleme sunar. Kapsayıcınızı kullanılabilirliğini, performansını ve kullanımını kolayca izleyebilir. Ayrıca hızlı bir şekilde tanımlamak ve bunları rapor bir kullanıcının bildirmesini beklemeden kapsayıcınızda hatalarını tanılayın.

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `ApplicationInsights` bölümü.

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `InstrumentationKey` | Dize | İzleme anahtarı Application Insights örneğinin hangi telemetri veri kapsayıcısı için gönderilir. Daha fazla bilgi için [ASP.NET Core için Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="authentication-configuration-settings"></a>Kimlik doğrulaması yapılandırma ayarları

`Authentication` Kapsayıcınız için Azure güvenlik seçenekleri yapılandırma ayarlarını belirtin. Bu bölüm yapılandırma ayarlarında kullanılabilir olsa da, bu bölümde metni tanı kapsayıcı kullanmaz.

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

`Billing` Yapılandırma ayarı, Azure üzerinde görüntü işleme kaynağın URI'sini kullanılan kapsayıcısı için fatura bilgileri ölçmek için uç nokta belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve Azure üzerinde bir görüntü işleme kaynak için geçerli bir uç noktası URI değeri olmalıdır.

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-configuration-setting) yapılandırma ayarlarını birlikte kullanılır ve üçünü de için geçerli değerler sağlamanız gerekir bunları; Aksi takdirde kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](computer-vision-how-to-install-containers.md#billing).

## <a name="eula-configuration-setting"></a>EULA'yı yapılandırma ayarı

`Eula` Yapılandırma ayarı, kapsayıcı lisansını kabul ettiğiniz belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer ayarlanmalıdır `accept`.

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-configuration-setting) yapılandırma ayarlarını birlikte kullanılır ve üçünü de için geçerli değerler sağlamanız gerekir bunları; Aksi takdirde kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](computer-vision-how-to-install-containers.md#billing).

Bilişsel hizmetler kapsayıcıları altında lisanslanır [sözleşmenize](https://go.microsoft.com/fwlink/?linkid=2018657) Azure kullanımınızı. Azure kullanımınızı var olan bir anlaşma değilse, Azure'un kullanımını düzenleyen sözleşmenize olduğunu kabul ediyorum [Microsoft çevrimiçi Abonelik Sözleşmesi](https://go.microsoft.com/fwlink/?linkid=2018755), kullanımımın [çevrimiçi hizmet koşulları ](https://go.microsoft.com/fwlink/?linkid=2018760). Önizlemeler için de kabul [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://go.microsoft.com/fwlink/?linkid=2018815). Kapsayıcı kullanarak bu koşulları kabul etmiş olursunuz.

## <a name="fluentd-configuration-settings"></a>Fluentd yapılandırma ayarları

`Fluentd` Bölümü için yapılandırma ayarlarını yönetir [Fluentd](https://www.fluentd.org), birleştirilmiş günlük kaydı için bir açık kaynak veri toplayıcısı. Bilgisayar işleme kapsayıcıları kapsayıcınızı günlüğüne yazmak sağlayan bir Fluentd oturum açma sağlayıcısı içerir ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `Fluentd` bölümü.

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Host` | Dize | Fluentd sunucusunun DNS ana bilgisayar adını veya IP adresi. |
| `Port` | Tamsayı | Fluentd sunucusunun bağlantı noktası.<br/> 24224 varsayılan değerdir. |
| `HeartbeatMs` | Tamsayı | Milisaniye cinsinden sinyal aralığı. Bu aralığı süresi dolmadan önce olay trafik gönderildi, bir sinyal Fluentd sunucuya gönderilir. 60000 milisaniye (1 dakika) varsayılan değerdir. |
| `SendBufferSize` | Tamsayı | Gönderme işlemleri için ayrılan bayt cinsinden ağ arabellek alanı. 32768 bayt (32 kilobayt) varsayılan değerdir. |
| `TlsConnectionEstablishmentTimeoutMs` | Tamsayı | Zaman aşımı, Fluentd sunucusuyla bir SSL/TLS bağlantı kurmak için milisaniye cinsinden. 10000 milisaniye (10 saniye) varsayılan değerdir.<br/> Varsa `UseTLS` yanlış olarak bu değeri yok sayıldı ayarlanmış. |
| `UseTLS` | Boole | Kapsayıcı Fluentd sunucusu ile iletişim kurmak için SSL/TLS kullanıp kullanmayacağını belirtir. Varsayılan değer false'tur. |


## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-configuration-settings"></a>Günlük kaydı yapılandırma ayarları

`Logging` Yapılandırma ayarlarını yönetmek kapsayıcınız için ASP.NET Core oturum açma desteği. Bir ASP.NET Core uygulaması için yapabileceğiniz kapsayıcınız için aynı yapılandırma ayarları ve değerleri kullanabilirsiniz. Aşağıdaki günlük kaydı sağlayıcıları, görüntü işleme kapsayıcıları tarafından desteklenir:

* [Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)  
  ASP.NET Core `Console` oturum açma sağlayıcısı. Tüm ASP.NET Core yapılandırma ayarlarını ve bu oturum açma sağlayıcısı için varsayılan değerleri desteklenir.
* [Hata ayıklama](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)  
  ASP.NET Core `Debug` oturum açma sağlayıcısı. Tüm ASP.NET Core yapılandırma ayarlarını ve bu oturum açma sağlayıcısı için varsayılan değerleri desteklenir.
* Disk  
  JSON oturum açma sağlayıcısı. Bu oturum açma sağlayıcısı için çıktı bağlama günlük verilerini yazar.  
  `Disk` Oturum açma sağlayıcısı, aşağıdaki yapılandırma ayarları destekler:  

  | Ad | Veri türü | Açıklama |
  |------|-----------|-------------|
  | `Format` | Dize | Günlük dosyaları için çıkış biçimi.<br/> **Not:** Bu değer ayarlanmalıdır `json` günlük sağlayıcısını etkinleştirmek için. Bu değer aynı zamanda bir kapsayıcı örneği oluşturulurken bir çıkış bağlama belirtmeden belirtilirse, bir hata oluşur. |
  | `MaxFileSize` | Tamsayı | Megabayt (MB) günlük dosyasının en büyük boyutu. Yeni bir günlük dosyası, geçerli günlük dosyası boyutunu karşıladığından veya bu değeri aşarsa, oturum açma sağlayıcısı tarafından başlatılır. -1 belirtilmezse, günlük dosyasının boyutu, çıkış bağlama yalnızca en büyük dosya boyutuyla sınırlıdır. Varsayılan değer 1’dir. |

ASP.NET Core günlük desteği yapılandırma hakkında daha fazla bilgi için bkz. [ASP.NET Core günlüğü](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#configuration).

## <a name="mounts-configuration-settings"></a>Yapılandırma ayarları bağlar

Görüntü işleme tarafından sağlanan Docker kapsayıcıları, durum bilgisiz ve sabit olacak şekilde tasarlanmıştır. Diğer bir deyişle, bir kapsayıcı içinde oluşturulan dosyalar kapsayıcı çalışıyor ve kolayca erişilemez, devam eden bir yazılabilir kapsayıcı katmanında depolanır. Bu kapsayıcı durdurulmuş veya kaldırılmış, onunla ilgili kapsayıcının içinde oluşturulan dosyalar yok edilir.

Ancak, Docker kapsayıcıları olduğunuzdan, birimler gibi Docker depolama seçenekleri kullanın ve okuma ve kapsayıcı destekliyorsa, kapsayıcısı dışındaki kalıcı veri yazma başlatmalar, bağlama. Belirtin ve Docker depolama seçenekleri yönetme hakkında daha fazla bilgi için bkz. [Docker yönetme](https://docs.docker.com/storage/).

> [!NOTE]
> Genellikle, bu yapılandırma ayarlarını değerlerini değiştirmek gerekmez. Bunun yerine, kapsayıcınız için girdi ve çıktı takar belirtirken, bu yapılandırma ayarlarını hedef olarak belirtilen değerleri kullanacaksınız. Giriş ve çıkış takar belirtme hakkında daha fazla bilgi için bkz. [giriş ve çıkış takar](#input-and-output-mounts).

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `Mounts` bölümü.

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Input` | Dize | Giriş bağlama hedefi. Varsayılan değer `/input`. |
| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer `/output`. |

### <a name="input-and-output-mounts"></a>Giriş ve çıkış başlatmalar

Varsayılan olarak, her kapsayıcı destekleyebilir bir *giriş bağlama*, hangi kapsayıcı verileri okuyabilir gelen ve *çıkış bağlama*, için hangi kapsayıcı veri yazabilirsiniz. Ancak, kapsayıcıları başlatmalar, çıkış veya giriş desteklemek için gerekli değildir ve her kapsayıcı, girdi ve çıktı takar görüntü işleme kapsayıcıları tarafından desteklenen günlük kaydı seçeneklerine ek olarak, kapsayıcı özgü amacıyla kullanabilirsiniz.

Metni Tanı kapsayıcı giriş takar desteklemiyor ve isteğe bağlı olarak destekler takar çıktı.

Bir giriş bağlama belirtin veya çıkış bağlama belirterek `--mount` seçeneğini [docker run](https://docs.docker.com/engine/reference/commandline/run/) indirilen kapsayıcı görüntüsünden bir kapsayıcı örneği oluşturmak için kullanılan komutu. Varsayılan olarak, giriş bağlama kullanımları `/input` hedefine ve çıktısını kullanan bağlama gibi `/output` hedefine olarak. Docker kapsayıcı konağı için kullanılabilen tüm Docker depolama seçeneği belirtilebilir `--mount` seçeneği.

Örneğin, aşağıdaki komutu için Docker bağlama bağlama tanımlar `D:\Output` klasörü çıkış bağlama olarak konak makinedeki sonra JSON biçiminde çıktı bağlama için günlük dosyalarını kaydetme metni tanı kapsayıcı görüntüsünden bir kapsayıcı oluşturur.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0 ApiKey=0123456789 Logging:Disk:Format=json
  ```

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../cognitive-services-container-support.md)