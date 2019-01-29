---
title: Kapsayıcıları yapılandırma
titlesuffix: Face - Azure Cognitive Services
description: Kapsayıcılar için yapılandırma ayarları.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: diberry
ms.openlocfilehash: 9e1d4ae38b18feb01d32ff62d4923b14d33494fa
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197679"
---
# <a name="configure-containers"></a>Kapsayıcıları yapılandırma

Yüz tanıma kapsayıcı ortak bir yapılandırma çerçeve kullanır, böylece kolayca yapılandırabilir ve depolama, günlüğe kaydetme ve telemetri ve güvenlik ayarları için kapsayıcılarınızı yönetin.

## <a name="configuration-settings"></a>Yapılandırma ayarları

Yüz tanıma kapsayıcı yapılandırma ayarlarında hiyerarşik ve tüm kapsayıcıları aşağıdaki üst düzey yapıya dayanarak, paylaşılan bir hiyerarşi:

* [ApiKey](#apikey-configuration-setting)
* [ApplicationInsights](#applicationinsights-configuration-settings)
* [Kimlik doğrulaması](#authentication-configuration-settings)
* [Faturalandırma](#billing-configuration-setting)
* [CloudAI](#cloudai-configuration-settings)
* [EULA'sı](#eula-configuration-setting)
* [Fluentd](#fluentd-configuration-settings)
* [HTTP proxy kimlik bilgileri ayarları](#http-proxy-credentials-settings)
* [Günlüğe kaydetme](#logging-configuration-settings)
* [Bağlar](#mounts-configuration-settings)

Kullanabilirsiniz [ortam değişkenlerini](#configuration-settings-as-environment-variables) veya [komut satırı bağımsız değişkenleri](#configuration-settings-as-command-line-arguments) yüz kapsayıcı örneği oluşturulurken yapılandırma ayarlarını belirtmek için.

Ortam değişkeni değerlerini, kapsayıcı görüntüsü için varsayılan değerleri sırayla geçersiz komut satırı bağımsız değişkeni değerlerini geçersiz kılar. Diğer bir deyişle, farklı değerler bir ortam değişkeni ve bir komut satırı bağımsız değişkeni aynı yapılandırma ayarı için aşağıdakiler gibi belirtirseniz `Logging:Disk:LogLevel`, ardından bir kapsayıcı örneği, ortam değişkeninin değeri kullanılır tarafından oluşturulmuş kapsayıcı.

### <a name="configuration-settings-as-environment-variables"></a>Ortam değişkenleri olarak yapılandırma ayarları

Kullanabileceğiniz [ASP.NET Core ortam değişkeni sözdizimini](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#configuration-by-environment) yapılandırma ayarlarını belirtmek için.

Kapsayıcı örneği oluşturulduğunda kapsayıcı kullanıcı ortam değişkenlerini okur. Bir ortam değişkeni varsa, ortam değişkeninin değerini belirtilen bir yapılandırma ayarı için varsayılan değeri geçersiz kılar. Ortam değişkenlerini kullanmanın avantajı, birden çok yapılandırma ayarları, kapsayıcı örneğini oluşturmadan önce ayarlanabilir ve birden çok kapsayıcı otomatik olarak aynı yapılandırma ayarları kümesini kullanabilirsiniz ' dir.

Örneğin, aşağıdaki komutları için konsol günlüğe kaydetme düzeyini yapılandırmak için bir ortam değişkenini kullanmak [LogLevel.Information](https://msdn.microsoft.com), ardından yüz kapsayıcı görüntüsünden bir kapsayıcı oluşturur. Ortam değişkeninin değerini, varsayılan yapılandırma ayarını geçersiz kılar.

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789
  ```

### <a name="configuration-settings-as-command-line-arguments"></a>Komut satırı bağımsız değişkenleri olarak yapılandırma ayarları

Kullanabileceğiniz [ASP.NET Core komut satırı bağımsız değişkeni sözdizimini](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#arguments) yapılandırma ayarlarını belirtmek için.

İsteğe bağlı yapılandırma ayarlarını belirtebilirsiniz `ARGS` parametresinin [docker run](https://docs.docker.com/engine/reference/commandline/run/) indirilen kapsayıcı görüntüsünden bir kapsayıcı örneği oluşturmak için kullanılan komutu. Her kapsayıcı farklı bir özel yapılandırma ayarları kümesini kullanabileceğiniz komut satırı bağımsız değişkenleri kullanmanın faydası olur.

Örneğin, aşağıdaki komutu, yüz tanıma kapsayıcı görüntüsünden bir kapsayıcı oluşturur ve varsayılan yapılandırma ayarı geçersiz kılma LogLevel.Information için günlük düzeyi konsol yapılandırır.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Console:LogLevel=Information
  ```

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

`ApiKey` Yapılandırma ayarı yüz kaynak yapılandırma anahtarı kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure üzerinde belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer için belirtilen yüz kaynak için geçerli yapılandırma anahtar olmalıdır [ `Billing` ](#billing-configuration-setting) yapılandırma ayarı.

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-configuration-setting) yapılandırma ayarlarını birlikte kullanılır ve üçünü de için geçerli değerler sağlamanız gerekir bunları; Aksi takdirde kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](face-how-to-install-containers.md#billing).

## <a name="applicationinsights-configuration-settings"></a>Applicationınsights yapılandırma ayarları

Yapılandırma ayarlarında `ApplicationInsights` bölümü eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği. Application Insights, kapsayıcınızın kod düzeyine derinlemesine izleme sunar. Kapsayıcınızı kullanılabilirliğini, performansını ve kullanımını kolayca izleyebilir. Ayrıca hızlı bir şekilde tanımlamak ve bunları rapor bir kullanıcının bildirmesini beklemeden kapsayıcınızda hatalarını tanılayın.

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `ApplicationInsights` bölümü.

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `InstrumentationKey` | Dize | İzleme anahtarı Application Insights örneğinin hangi telemetri veri kapsayıcısı için gönderilir. Daha fazla bilgi için [ASP.NET Core için Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="authentication-configuration-settings"></a>Kimlik doğrulaması yapılandırma ayarları

`Authentication` Kapsayıcınız için Azure güvenlik seçenekleri yapılandırma ayarlarını belirtin. Bu bölüm yapılandırma ayarlarında kullanılabilir olsa da, bu bölümde yüz kapsayıcı kullanmaz.

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Storage` | Dize | İzleme anahtarı Application Insights örneğinin hangi telemetri veri kapsayıcısı için gönderilir. Daha fazla bilgi için [ASP.NET Core için Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

`Billing` Yapılandırma ayarı, Azure üzerinde yüz tanıma kaynağın URI'sini kullanılan kapsayıcısı için fatura bilgileri ölçmek için uç nokta belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve Azure üzerinde bir yüz kaynak için geçerli bir uç noktası URI değeri olmalıdır.

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-configuration-setting) yapılandırma ayarlarını birlikte kullanılır ve üçünü de için geçerli değerler sağlamanız gerekir bunları; Aksi takdirde kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](face-how-to-install-containers.md#billing).

## <a name="cloudai-configuration-settings"></a>CloudAI yapılandırma ayarları

Yapılandırma ayarlarında `CloudAI` bölümü kapsayıcınıza benzersiz kapsayıcı özgü seçenekler sağlar. Aşağıdaki ayarları ve nesneleri yüz kapsayıcıda desteklenir `CloudAI` bölümü

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Storage` | Nesne | Yüz tanıma kapsayıcı tarafından kullanılan depolama senaryo. Depolama senaryoları ve için ilişkili ayarları hakkında daha fazla bilgi için `Storage` nesne, bkz: [depolama senaryo ayarları](#storage-scenario-settings) |

### <a name="storage-scenario-settings"></a>Depolama senaryosu ayarları

Yüz tanıma kapsayıcı, blob, önbellek, meta verileri ve sıra veri, ne saklanan bağlı olarak depolar. Örneğin, eğitim dizinleri ve sonuçları bir büyük kişi grubu için blob veri depolanır. Yüz tanıma kapsayıcı ile etkileşim kurulurken iki farklı depolama senaryoları ve bu tür verilerin depolama sağlar:

* Bellek  
  Tüm dört veri türlerini bellekte depolanır. Dağılımı ya da bunların kalıcıdır. Yüz tanıma kapsayıcı durdurulmuş veya kaldırılmış, tüm bu kapsayıcı için depolama birimindeki verileri bozulur.  
  Yüz tanıma kapsayıcısı için varsayılan depolama senaryosu budur.
* Azure  
  Yüz tanıma kapsayıcı, bu dört veri türleri arasında kalıcı depolama dağıtmak için Azure depolama ve Azure Cosmos DB kullanır. BLOB ve kuyruk verileri Azure Depolama tarafından işlenir. Meta verileri ve önbellek verilerini Azure Cosmos DB tarafından işlenir. Yüz tanıma kapsayıcı durdurulmuş veya kaldırılmış, tüm veriler bu kapsayıcı için depolama alanında kalır Azure depolama ve Azure Cosmos DB içinde depolanan.  
  Azure depolama senaryo tarafından kullanılan kaynakları aşağıdaki ek gereksinimlere sahiptir.
  * Azure depolama kaynağı StorageV2 hesap türü kullanmalısınız.
  * Azure Cosmos DB kaynak MongoDB için Azure Cosmos DB'nin API'sini kullanmanız gerekir

Tarafından yönetilen depolama senaryolar ve ilişkili yapılandırma ayarları `Storage` altında nesne `CloudAI` yapılandırma bölümü. Aşağıdaki yapılandırma ayarları kullanılabilir `Storage` nesnesi:

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `StorageScenario` | Dize | Kapsayıcı tarafından desteklenen depolama senaryo. Aşağıdaki değerler kullanılabilir<br/>`Memory` -Varsayılan değer. Kapsayıcı, tek bir düğüm, geçici kullanım için kalıcı olmayan, olmayan dağıtılmış ve bellek içi depolama kullanır. Bu kapsayıcı için depolama kapsayıcısı durdurulmuş veya kaldırılmış olmadığını yok edilir.<br/>`Azure` -Kapsayıcı, depolama için Azure kaynaklarını kullanır. Bu kapsayıcı için depolama kapsayıcısı durdurulmuş veya kaldırılmış olmadığını kalıcı hale getirilir.|
| `ConnectionStringOfAzureStorage` | Dize | Kapsayıcı tarafından kullanılan Azure depolama kaynağı için bağlantı dizesi.<br/>Bu ayar, yalnızca aşağıdaki durumlarda geçerlidir `Azure` için belirtilen `StorageScenario` yapılandırma ayarı. |
| `ConnectionStringOfCosmosMongo` | Dize | Kapsayıcı tarafından kullanılan Azure Cosmos DB kaynak için MongoDB bağlantı dizesi.<br/>Bu ayar, yalnızca aşağıdaki durumlarda geçerlidir `Azure` için belirtilen `StorageScenario` yapılandırma ayarı. |

Örneğin, aşağıdaki komutu, Azure depolama senaryosu belirtir ve örnek bağlantı dizeleri yüz kapsayıcı verilerini depolamak için kullanılan Azure depolama ve Cosmos DB kaynakları sağlar.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

Depolama senaryosu alanından ayrı olarak yönetilir takar giriş ve çıkış takar. Tek bir kapsayıcı için bu özellikleri birleşimi belirtebilirsiniz. Örneğin, aşağıdaki komutu için Docker bağlama bağlama tanımlar `D:\Output` klasörü çıkış bağlama olarak konak makinedeki sonra JSON biçiminde çıktı bağlama için günlük dosyalarını kaydetme yüz kapsayıcı görüntüsünden bir kapsayıcı oluşturur. Komut ayrıca Azure depolama senaryosu belirtir ve örnek bağlantı dizeleri yüz kapsayıcı verilerini depolamak için kullanılan Azure depolama ve Cosmos DB kaynakları sağlar.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

## <a name="eula-configuration-setting"></a>EULA'yı yapılandırma ayarı

`Eula` Yapılandırma ayarı, kapsayıcı lisansını kabul ettiğiniz belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer ayarlanmalıdır `accept`.

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-configuration-setting) yapılandırma ayarlarını birlikte kullanılır ve üçünü de için geçerli değerler sağlamanız gerekir bunları; Aksi takdirde kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](face-how-to-install-containers.md#billing).

Bilişsel hizmetler kapsayıcıları altında lisanslanır [sözleşmenize](https://go.microsoft.com/fwlink/?linkid=2018657) Azure kullanımınızı. Azure kullanımınızı var olan bir anlaşma değilse, Azure'un kullanımını düzenleyen sözleşmenize olduğunu kabul ediyorum [Microsoft çevrimiçi Abonelik Sözleşmesi](https://go.microsoft.com/fwlink/?linkid=2018755) (kullanımımın [çevrimiçi hizmet koşulları ](https://go.microsoft.com/fwlink/?linkid=2018760)). Önizlemeler için de kabul [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://go.microsoft.com/fwlink/?linkid=2018815). Kapsayıcı kullanarak bu koşulları kabul etmiş olursunuz.

## <a name="fluentd-configuration-settings"></a>Fluentd yapılandırma ayarları

`Fluentd` Bölümü için yapılandırma ayarlarını yönetir [Fluentd](https://www.fluentd.org), birleştirilmiş günlük kaydı için bir açık kaynak veri toplayıcısı. Yüz tanıma kapsayıcı kapsayıcınızı günlüğüne yazmak sağlayan bir Fluentd oturum açma sağlayıcısı içerir ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.

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

`Logging` Yapılandırma ayarlarını yönetmek kapsayıcınız için ASP.NET Core oturum açma desteği. Bir ASP.NET Core uygulaması için yapabileceğiniz kapsayıcınız için aynı yapılandırma ayarları ve değerleri kullanabilirsiniz. Aşağıdaki günlük kaydı sağlayıcıları yüz kapsayıcı tarafından desteklenir:

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
  | `MaxFileSize` | Tamsayı | Megabayt (MB) günlük dosyasının en büyük boyutu. Yeni bir günlük dosyası, geçerli günlük dosyası boyutunu karşıladığından veya bu değeri aşarsa, oturum açma sağlayıcısı tarafından başlatılır. -1 belirtilmezse, günlük dosyasının boyutu, çıkış bağlama yalnızca en büyük dosya boyutuyla sınırlıdır. Varsayılan değer 1'dir. |

ASP.NET Core günlük desteği yapılandırma hakkında daha fazla bilgi için bkz. [ayarları dosya Yapılandırması](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1).

## <a name="mounts-configuration-settings"></a>Yapılandırma ayarları bağlar

Yüz tanıma tarafından sağlanan Docker kapsayıcıları, durum bilgisiz ve sabit olacak şekilde tasarlanmıştır. Diğer bir deyişle, bir kapsayıcı içinde oluşturulan dosyalar kapsayıcı çalışıyor ve kolayca erişilemez, devam eden bir yazılabilir kapsayıcı katmanında depolanır. Bu kapsayıcı durdurulmuş veya kaldırılmış, onunla ilgili kapsayıcının içinde oluşturulan dosyalar yok edilir.

Ancak, Docker kapsayıcıları olduğunuzdan, birimler gibi Docker depolama seçenekleri kullanın ve okuma ve kapsayıcı destekliyorsa, kapsayıcısı dışındaki kalıcı veri yazma başlatmalar, bağlama. Belirtin ve Docker depolama seçenekleri yönetme hakkında daha fazla bilgi için bkz. [Docker yönetme](https://docs.docker.com/storage/).

> [!NOTE]
> Genellikle, bu yapılandırma ayarlarını değerlerini değiştirmek gerekmez. Bunun yerine, kapsayıcınız için girdi ve çıktı takar belirtirken, bu yapılandırma ayarlarını hedef olarak belirtilen değerleri kullanacaksınız. Giriş ve çıkış takar belirtme hakkında daha fazla bilgi için bkz. [giriş ve çıkış takar](#input-and-output-mounts).

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `Mounts` bölümü.

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Input` | Dize | Giriş bağlama hedefi. Varsayılan değer `/input`. |
| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer `/output`. |

### <a name="input-and-output-mounts"></a>Giriş ve çıkış başlatmalar

Varsayılan olarak, her kapsayıcı destekleyebilir bir *giriş bağlama*, hangi kapsayıcı verileri okuyabilir gelen ve *çıkış bağlama*, için hangi kapsayıcı veri yazabilirsiniz. Ancak, kapsayıcıları başlatmalar, çıkış veya giriş desteklemek için gerekli değildir ve her kapsayıcı, girdi ve çıktı takar yüz kapsayıcı tarafından desteklenen günlük kaydı seçeneklerine ek olarak, kapsayıcı özgü amacıyla kullanabilirsiniz.

Yüz kapsayıcı giriş takar desteklemiyor ve isteğe bağlı olarak destekler takar çıktı.

Bir giriş bağlama belirtin veya çıkış bağlama belirterek `--mount` seçeneğini [docker run](https://docs.docker.com/engine/reference/commandline/run/) indirilen kapsayıcı görüntüsünden bir kapsayıcı örneği oluşturmak için kullanılan komutu. Varsayılan olarak, giriş bağlama kullanımları `/input` hedefine ve çıktısını kullanan bağlama gibi `/output` hedefine olarak. Docker kapsayıcı konağı için kullanılabilen tüm Docker depolama seçeneği belirtilebilir `--mount` seçeneği.

Örneğin, aşağıdaki komutu için Docker bağlama bağlama tanımlar `D:\Output` klasörü çıkış bağlama olarak konak makinedeki sonra JSON biçiminde çıktı bağlama için günlük dosyalarını kaydetme yüz kapsayıcı görüntüsünden bir kapsayıcı oluşturur.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json
  ```

Yüz kapsayıcı giriş kullanmaz ya da eğitim veya veritabanı verileri depolamak için çıkış bağlar. Bunun yerine, yüz tanıma kapsayıcı, eğitim ve veritabanı verileri yönetmek için depolama senaryoları sağlar. Depolama senaryoları kullanma hakkında daha fazla bilgi için bkz. [depolama senaryo ayarları](#storage-scenario-settings).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../cognitive-services-container-support.md)