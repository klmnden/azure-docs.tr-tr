---
title: Yükleme ve kapsayıcı - Form tanıyıcı çalıştırma
titleSuffix: Azure Cognitive Services
description: Form ve tablo verisini ayrıştırmak için Form Tanıma kapsayıcısını kullanmayı öğrenin.
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: f38752928832b7dee6a7e55f1d25374a64391bbe
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441891"
---
# <a name="install-and-run-form-recognizer-containers"></a>Yüklemek ve forma tanıyıcı kapsayıcılarını çalıştırın
Azure Form tanıyıcı tanımlamak ve anahtar-değer çiftleri ve tabloları formlardan ayıklamak için makine öğrenimi teknolojisi geçerlidir. Bu değerler ve tablo girişleri anahtar-değer çiftleri ile ilişkilendirir ve ardından özgün dosyayı ilişkileri içeren yapılandırılmış verileri çıkarır. 

Karmaşıklığı azaltın ve iş akışı Otomasyonu işleminizi veya başka bir uygulama özel bir formu tanıyıcı model kolayca tümleştirmenize için basit bir REST API kullanarak model çağırabilirsiniz. Yalnızca beş form belge (veya boş bir form ve doldurulmuş iki biçimi) sonuçları alabilmeniz için hızlı bir şekilde, doğru bir şekilde, gerekli ve belirli içeriğinizi uyarlanmış. Hiçbir ağır el ile müdahale veya kapsamlı veri bilimi uzmanlığına gereklidir. Ve veri etiketleme veya veri ek açıklama gerektirmez.

|İşlev|Özellikler|
|-|-|
|Form Tanıma| <li>PDF, PNG ve JPG dosyaları işleme<li>En az 5 forms aynı düzeni trenler özel modelleri <li>Anahtar-değer çiftleri ve tablo bilgilerini ayıklar. <li>Algılama ve görüntülerinden formları içindeki yazdırılan metin ayıklamak için Azure Bilişsel hizmetler bilgisayar işleme API metin tanıma özelliğini kullanır<li>Ek açıklama veya etiketleme gerektirmez|

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Form tanıyıcı kapsayıcıları kullanmadan önce aşağıdaki önkoşulları sağlamanız gereklidir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker Altyapısı'nın kurulu ihtiyacınız bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> Windows Docker Linux kapsayıcıları desteklemek için de yapılandırılması gerekir.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntüleri ve temel bilgi gibi olmalıdır `docker` komutları.|
|The Azure CLI| Yükleme [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) konağınızdaki.|
|Görüntü işleme API'si kaynak bilgisayar| Taranan belgeleri ve resimleri işlemek için bir görüntü işleme kaynak gerekir. Metin tanıma özelliği ya da bir Azure kaynağı (REST API veya SDK'sı) erişebilir veya *bilişsel hizmetler-tanıma-metin* [kapsayıcı](../Computer-vision/computer-vision-how-to-install-containers.md##get-the-container-image-with-docker-pull). Her zamanki faturalandırma ücretleri uygulanır. <br><br>Hem anahtar hem de fatura uç noktaları, görüntü işleme kaynak için (Azure Bulut veya Bilişsel hizmetler kapsayıcısı) geçirin. Bu anahtar ve fatura uç nokta {COMPUTER_VISION_API_KEY} kullanın ve {COMPUTER_VISION_BILLING_ENDPOINT_URI}.<br><br> Kullanırsanız *bilişsel hizmetler-tanıma-metin* kapsayıcı emin olun:<br><br>Görüntü işleme anahtarınız Form tanıyıcı kapsayıcısı için görüntü işleme belirtilen anahtarla ise `docker run` komutunu *bilişsel hizmetler-tanıma-metin* kapsayıcı.<br>Fatura uç noktanızı kapsayıcının uç noktadır (örneğin, `https://localhost:5000`). Görüntü işleme kapsayıcı ve Form tanıyıcı kapsayıcı birlikte aynı ana bilgisayarda kullanırsanız, bunlar her ikisi de varsayılan bağlantı noktası ile başlatılamıyor *5000*.  |  
|Form tanıyıcı kaynağı |Bu kapsayıcılar kullanmak için şunlara sahip olmalısınız:<br><br>A _Form tanıyıcı_ fatura uç noktası URI'si ve ilişkili faturalandırma anahtarı almak için Azure kaynak. Hem Azure portalında kullanılabilir değerler **Form tanıyıcı genel bakış** ve **Form tanıyıcı genel bakış anahtarları** sayfaları ve her iki değer kapsayıcı başlatma için gereklidir.<br><br>**{BILLING_KEY}** : kaynak anahtarı<br><br>**{BILLING_ENDPOINT_URI}** : uç nokta URI'si örneğidir `https://westus.api.cognitive.microsoft.com/forms/v1.0`| 

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteği

Önce tamamlamanız ve gönderme gerekir [Bilişsel Hizmetleri Form tanıyıcı kapsayıcılara erişimine istek formunu](https://aka.ms/FormRecognizerRequestAccess) kapsayıcıya erişim istemek için. Bunun yapılması için görüntü işleme, imzalar. Görüntü işleme istek formu için ayrı ayrı oturum gerek yoktur. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, en düşük ve önerilen CPU Çekirdeği ve her bir Form tanıyıcı kapsayıcısı için ayrılacak bellek açıklanmıştır:

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
|bilişsel-services-form-tanıyıcı | 2 Çekirdek, 4 GB bellek | 4 çekirdek, 8 GB bellek |

* Her çekirdeğe en az 2.6 gigahertz (GHz) olması ya da daha hızlı.
* TPS - saniye başına işlem
* Çekirdek ve bellek karşılık `--cpus` ve `--memory` parçası olarak kullanılan ayarları `docker run` komutu.

> [!Note]
> En düşük ve önerilen değerler Docker barındırabileceğiniz temel alır ve *değil* ana makine kaynakları.

## <a name="get-the-container-image-with-the-docker-pull-command"></a>Docker pull komutuyla kapsayıcı görüntüsünü Al

Form tanıyıcı için kapsayıcı görüntülerini aşağıdaki depoda kullanılabilir:

| Kapsayıcı | Havuz |
|-----------|------------|
| bilişsel-services-form-tanıyıcı | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:latest` |

Kullanmayı planlıyorsanız `cognitive-services-recognize-text` [kapsayıcı](../Computer-vision/computer-vision-how-to-install-containers.md##get-the-container-image-with-docker-pull), Form tanıyıcı hizmet yerine kullandığınızdan emin olun `docker pull` komutunu doğru kapsayıcı adı: 

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-form-recognizer-container"></a>Docker isteği formu tanıyıcı kapsayıcısı için

#### <a name="form-recognizer"></a>Form Tanıma

Form tanıyıcı kapsayıcı almak için aşağıdaki komutu kullanın:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:latest
```

## <a name="how-to-use-the-container"></a>Kapsayıcı kullanma

Kapsayıcı üzerinde sonra [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırmak](#run-the-container-by-using-the-docker-run-command), gerekli, ancak kullanılmadı faturalama ayarları. Daha fazla [örnekler](form-recognizer-container-configuration.md#example-docker-run-commands) , `docker run` komutu kullanılabilir.
1. [Kapsayıcının tahmini uç nokta sorgu](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-by-using-the-docker-run-command"></a>Komutu çalıştırarak docker'ı kullanarak kapsayıcı çalıştırma

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) üç kapsayıcı birini çalıştırmak için komutu. Komutu şu parametreleri kullanır:

| Yer tutucu | Değer |
|-------------|-------|
|{BILLING_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır. Azure portalında kullanılabilir **Form tanıyıcı anahtarları** sayfası.  |
|{BILLING_ENDPOINT_URI} | Fatura uç noktası URI değeri Azure Portal'da kullanılabilir **Form tanıyıcı genel bakış** sayfası.|
|{COMPUTER_VISION_API_KEY}| Anahtar Azure portalında kullanılabilir **bilgisayar işleme API anahtarları** sayfası.|
|{COMPUTER_VISION_ENDPOINT_URI}|Fatura uç noktası. Bulut tabanlı bir görüntü işleme kaynak kullanıyorsanız, URI değeri Azure Portal'da kullanılabilir **bilgisayar işleme API'sine genel bakış** sayfası. Kullanıyorsanız, bir `cognitive-services-recognize-text` kapsayıcı, kapsayıcıda geçirilir fatura uç nokta URL'sini kullanın `docker run` komutu.|

Bu parametreleri aşağıdaki örnekte kendi değerlerinizle değiştirin `docker run` komutu.

### <a name="form-recognizer"></a>Form Tanıma

```bash
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

Bu komut:

* Bir Form tanıyıcı kapsayıcı kapsayıcı görüntüsünü çalıştırır.
* 2 CPU Çekirdeği ve 8 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır.
* Bir/Output birim kapsayıcısı ve bir /input bağlar.

[!INCLUDE [Running multiple containers on the same host H2](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

### <a name="run-separate-containers-as-separate-docker-run-commands"></a>Ayrı kapsayıcıları ayrı docker komutlarını çalıştırmak çalıştırın

Aynı ana bilgisayarda yerel olarak barındırılan Form tanıyıcı ve metin tanıyıcı birleşimi için aşağıdaki iki örnek Docker CLI komutları kullanın:

İlk kapsayıcı 5000 numaralı çalıştırın. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

İkinci kapsayıcıyı 5001 bağlantı noktası üzerinde çalıştırın.


```bash 
docker run --rm -it -p 5001:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={COMPUTER_VISION_ENDPOINT_URI} \
ApiKey={COMPUTER_VISION_API_KEY}
```
Sonraki her kapsayıcı farklı bir bağlantı noktası olmalıdır. 

### <a name="run-separate-containers-with-docker-compose"></a>Ayrı kapsayıcıları, Docker Compose ile çalıştırın

Aynı ana bilgisayarda yerel olarak barındırılan Form tanıyıcı ve metin tanıyıcı birleşimi için aşağıdaki örnekte Docker Compose YAML dosyasına bakın. Metin tanıyıcı `{COMPUTER_VISION_API_KEY}` her ikisi için aynı olmalıdır `formrecognizer` ve `ocr` kapsayıcıları. `{COMPUTER_VISION_ENDPOINT_URI}` Yalnızca kullanılan `ocr` kapsayıcı, çünkü `formrecognizer` kapsayıcı kullanır `ocr` adı ve bağlantı noktası. 

```docker
version: '3.3'
services:   
  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 8g
        reservations:
          cpus: '1'
          memory: 4g
    environment:
      eula: accept
      billing: "{COMPUTER_VISION_ENDPOINT_URI}"
      apikey: {COMPUTER_VISION_API_KEY}  

  formrecognizer:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 8g
        reservations:
          cpus: '1'
          memory: 4g
    environment:
      eula: accept
      billing: "{BILLING_ENDPOINT_URI}"
      apikey: {BILLING_KEY}
      FormRecognizer__ComputerVisionApiKey: {COMPUTER_VISION_API_KEY}
      FormRecognizer__ComputerVisionEndpointUri: "http://ocr:5000"
      FormRecognizer__SyncProcessTaskCancelLimitInSecs: 75
    links:
      - ocr
    volumes:
      - type: bind
        source: c:\output
        target: /output
      - type: bind
        source: c:\input
        target: /input
    ports:
      - "5000:5000"  
```


> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey`, hem de `FormRecognizer:ComputerVisionApiKey` ve `FormRecognizer:ComputerVisionEndpointUri` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor. Daha fazla bilgi için [faturalama](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

|Kapsayıcı|Uç Nokta|
|--|--|
|Form tanıyıcı|http://localhost:5000


### <a name="form-recognizer"></a>Form Tanıma

Kapsayıcı websocket tabanlı sorgu uç noktası aracılığıyla erişilen API'ler sağlar [Form tanıyıcı Hizmetleri SDK Belgeleri](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/).

Varsayılan olarak, Form tanıyıcı SDK'sı Çevrimiçi Hizmetleri kullanır. Kapsayıcı kullanmak için başlatma yöntemi değiştirmeniz gerekir. Aşağıdaki örneklere bakın.

#### <a name="for-c"></a>İçinC#

Bu Azure bulut başlatma çağrısı kullanarak değiştirin:

```C#
var config = FormRecognizerConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

Bu çağrı, kapsayıcı uç noktasını kullanır:

```C#
var config = FormRecognizerConfig.FromEndpoint("ws://localhost:5000/formrecognizer/v1.0-preview/custom", "YourSubscriptionKey");
```

#### <a name="for-python"></a>Python için

Bu Azure bulut başlatma çağrısı kullanarak değiştirin:

```python
formrecognizer_config = formrecognizersdk.FormRecognizerConfig(subscription=formrecognizer_key, region=service_region)
```

Bu çağrı, kapsayıcı uç noktasını kullanır:

```python
formrecognizer_config = formrecognizersdk.FormRecognizerConfig(subscription=formrecognizer_key, endpoint="ws://localhost:5000/formrecognizer/v1.0-preview/custom"
```

### <a name="form-recognizer"></a>Form Tanıma

REST uç noktasını bulabileceğiniz API'leri, kapsayıcı sağlar [Form tanıyıcı API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeWithCustomModel) sayfası.


[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]


## <a name="stop-the-container"></a>Kapsayıcı Durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcı çalıştırdığınızda, kapsayıcının kullanan **stdout** ve **stderr** başlattığınızda veya kapsayıcı çalıştırmanızın ortaya çıkan sorunları gidermede yardımcı olan çıkış bilgiler.

## <a name="billing"></a>Faturalandırma

Form tanıyıcı kapsayıcıları faturalandırma bilgileri kullanarak Azure'a gönderin bir _Form tanıyıcı_ Azure hesabınız kaynaktaki.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](form-recognizer-container-configuration.md).

<!--blogs/samples/video courses -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve Form tanıyıcı kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Form tanıyıcı bir Linux kapsayıcı için Docker sağlar.
* Azure'da özel kapsayıcı kayıt defterinden kapsayıcı görüntülerini indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Ana kapsayıcısının URI belirterek Form tanıyıcı kapsayıcısında işlemleri çağırmak için REST API veya REST SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği, faturalandırma bilgileri belirtmeniz gerekir.

> [!IMPORTANT]
>  Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](form-recognizer-container-configuration.md) yapılandırma ayarları için.
* Daha fazla kullanmanız [Bilişsel hizmet kapsayıcılarında](../cognitive-services-container-support.md).
