---
title: Yükleme ve Anomali algılayıcısı API kullanımı için kapsayıcıları çalıştırın
titleSuffix: Azure Cognitive Services
description: Zaman serisi verilerinizdeki anormallikleri belirlemek için gelişmiş algoritmalar Anomali algılayıcısı API'nin kullanın.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: f84f1bab048630d6dd45085b3d082004d10bb6a8
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721713"
---
# <a name="install-and-run-anomaly-detector-containers"></a>Yükleme ve Anomali algılayıcısı kapsayıcıları çalıştırma

Anomali algılayıcısı şu kapsayıcıyı sahiptir: 

|İşlev|Özellikler|
|-|-|
|Anomali Algılayıcısı| <li> Anormallikleri gerçek zamanlı olarak oluşunca algılar. <li> Toplu olarak veri kümenizi boyunca anomalileri algılar. <li> Verilerinizin beklenen normal aralık çıkarır. <li> Daha iyi destekleyen anomali algılama duyarlılığı ayarlama verilerinize. |

API'leri hakkında ayrıntılı bilgi için bkz:
* [Anomali algılayıcısı API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Anomali algılayıcısı kapsayıcıları kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker Altyapısı'nın kurulu ihtiyacınız bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows üzerinde**, Docker de Linux kapsayıcıları destekler şekilde yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.| 
|Anomali algılayıcısı kaynak |Bu kapsayıcıların kullanabilmeniz için şunlara sahip olmalısınız:<br><br>Bir _Anomali algılayıcısı_ fatura uç noktası URI'si ve ilişkili faturalandırma anahtarı almak için Azure kaynak. Her iki değeri de Azure portalının Anomali algılayıcısı genel bakış ve anahtarları sayfalarında kullanılabilir ve kapsayıcı başlatma için gereklidir.<br><br>**{BILLING_KEY}** : kaynak anahtarı<br><br>**{BILLING_ENDPOINT_URI}** : uç nokta URI'si örnektir: `https://westus2.api.cognitive.microsoft.com`|

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteği

Önce tamamlamanız ve gönderme gerekir [Anomali algılayıcısı kapsayıcı istek formunu](https://aka.ms/adcontainer) kapsayıcıya erişim istemek için.

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

<!--* [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/). For instructions of deploying Anomaly Detector module in IoT Edge, see [How to deploy Anomaly Detector module in IoT Edge](how-to-deploy-anomaly-detector-module-in-iot-edge.md).-->

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, en düşük ve önerilen CPU Çekirdeği ve Anomali algılayıcısı kapsayıcı için ayrılacak bellek açıklanmaktadır.

| QPS (Saniyedeki sorgu sayısı) | Minimum | Önerilen |
|-----------|---------|-------------|
| 10 QPS | 4 çekirdek, 1GB bellek | 8 çekirdek 2GB bellek |
| 20 QPS | 8 çekirdek, 2GB bellek | 16 çekirdek 4GB bellek |

Her çekirdeğe en az 2.6 gigahertz (GHz) olması ya da daha hızlı.

Çekirdek ve bellek karşılık `--cpus` ve `--memory` parçası olarak kullanılan ayarları `docker run` komutu.

## <a name="get-the-container-image-with-docker-pull"></a>İle kapsayıcı görüntüsünü Al `docker pull`

Kullanım [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) komutu, kapsayıcı görüntüsü indirilemedi.

| Kapsayıcı | Havuz |
|-----------|------------|
| bilişsel-services-anomali-algılayıcısı | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest` |

<!--
For a full description of available tags, such as `latest` used in the preceding command, see [anomaly-detector](https://go.microsoft.com/fwlink/?linkid=2083827&clcid=0x409) on Docker Hub.
-->
[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]


### <a name="docker-pull-for-the-anomaly-detector-container"></a>Docker isteği Anomali algılayıcısı kapsayıcısı için

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest
```

## <a name="how-to-use-the-container"></a>Kapsayıcı kullanma

Kapsayıcı açıldığında [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırmak](#run-the-container-with-docker-run), gerekli faturalama ayarları. Daha fazla [örnekler](anomaly-detector-container-configuration.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 
1. [Kapsayıcının tahmini uç nokta sorgu](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcı ile çalıştırma `docker run`

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) üç kapsayıcı birini çalıştırmak için komutu. Komutu şu parametreleri kullanır:

| Yer tutucu | Değer |
|-------------|-------|
|{BILLING_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır ve Azure portalının Anomali algılayıcısı anahtarlar sayfasında bulabilirsiniz.  |
|{BILLING_ENDPOINT_URI} | Fatura uç noktası URI değerini Azure portalının Anomali algılayıcısı genel bakış sayfasında kullanılabilir.|

Bu parametreleri aşağıdaki örnekte kendi değerlerinizle değiştirin `docker run` komutu.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Bu komut:

* Kapsayıcı görüntüsünü Anomali algılayıcısı kapsayıcı çalıştırır
* Bir CPU Çekirdeği ve 4 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

### <a name="running-multiple-containers-on-the-same-host"></a>Aynı ana bilgisayarda çalışan birden fazla kapsayıcılar

Kullanıma sunulan bağlantı noktası olan birden çok kapsayıcı çalıştırmak istiyorsanız, her kapsayıcı farklı bir bağlantı noktası ile çalıştırmak emin olun. Örneğin, ilk kapsayıcı bağlantı noktası 5000 ve ikinci kapsayıcıyı 5001 bağlantı noktası üzerinde çalıştırın.

Değiştirin `<container-registry>` ve `<container-name>` kullandığınız kapsayıcıları değerleri. Bunlar aynı kapsayıcı olması gerekmez. Anomali algılayıcısı kapsayıcısı ve LUIS kapsayıcı birlikte çalışıyor olabilir veya çalışan birden çok Anomali algılayıcısı kapsayıcılar sahip olabilir. 

İlk kapsayıcı 5000 numaralı çalıştırın. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

İkinci kapsayıcıyı 5001 bağlantı noktası üzerinde çalıştırın.


```bash 
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Sonraki her kapsayıcı farklı bir bağlantı noktası olmalıdır. 

## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

Kapsayıcı, REST tabanlı sorgu tahmin uç nokta API'leri sağlar. 

Ana bilgisayarını kullanmak https://localhost:5000, kapsayıcı API'leri için.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcı Durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcı içeren bir çıktı çalıştırırsanız [bağlama](anomaly-detector-container-configuration.md#mount-settings) ve günlüğe kaydetme etkin, kapsayıcı başlatma veya kapsayıcı çalıştırma sırasında gerçekleşen sorunları gidermek yararlı olan günlük dosyalarını oluşturur. 

## <a name="billing"></a>Faturalandırma

Kullanarak, fatura bilgilerini Azure Anomali algılayıcısı kapsayıcıları Gönder bir _Anomali algılayıcısı_ Azure hesabınız kaynaktaki. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](anomaly-detector-container-configuration.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve çalışan Anomali algılayıcısı kapsayıcılar için iş akışı öğrendiniz. Özet:

* Anomali algılayıcısı, Docker için anomali algılama akış, beklenen aralığın çıkarımı ve duyarlılık ayarlama batch vs ile şifrelenmiş bir Linux kapsayıcı sağlar.
* Kapsayıcı görüntülerini bir özel Azure Container registry'den kapsayıcı Önizleme için adanmış indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Ana kapsayıcısının URI belirterek Anomali algılayıcısı kapsayıcılarında işlemleri çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (analiz ediliyor. Örneğin, zaman serisi verileri) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](anomaly-detector-container-configuration.md) yapılandırma ayarları
* [Anomali algılayıcısı API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
