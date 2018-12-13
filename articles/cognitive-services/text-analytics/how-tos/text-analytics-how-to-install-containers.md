---
title: Kapsayıcıları yükleme ve çalıştırma
titleSuffix: Text Analytics -  Azure Cognitive Services
description: İndirme, yükleme ve bu izlenecek yol öğreticide metin analizi için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 11798c3bfd4032ad10c738032a816a2a0488ce67
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090542"
---
# <a name="install-and-run-containers"></a>Kapsayıcıları yükleme ve çalıştırma

Kapsayıcı, bir uygulama veya hizmet bir kapsayıcı görüntüsüne paketlenmiştir yazılım dağıtım için kullanılan bir yaklaşımdır. Yapılandırma ve bağımlılıklarla uygulama veya hizmet için kapsayıcı görüntüsüne eklenir. Kapsayıcı görüntüsü, bir kapsayıcı konağında çok az kayıpla veya hiç değişiklik ile sonra dağıtılabilir. Birbirine ve bir sanal makine değerinden daha küçük bir kaplama alanı ile temel işletim sistemi, yalıtılmış kapsayıcılardır. Kapsayıcılar için kısa vadeli görevleri kapsayıcı görüntülerinden oluşturulan ve artık gerekli olmadığında kaldırıldı.

Metin analizi, her biri işlevlerinin bir alt kümesini içeren Docker kapsayıcıları, aşağıdaki kümesini sağlar:

| Kapsayıcı| Açıklama |
|----------|-------------|
|Anahtar İfade Ayıklama | Ana noktaları belirleyin, anahtar ifadeleri ayıklar. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür. |
|Dil Algılama | En fazla 120 dil için algılar ve raporları giriş metni hangi dilde yazılır. Kapsayıcı, istekte bulunan her belge için bir tek dil kodu bildirir. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir. |
|Duygu Analizi | Ham metin pozitif veya negatif yaklaşım hakkında ipuçları için analiz eder. API, her belge için 0 ile 1 arasında bir yaklaşım puanı döndürür ve 1 en pozitif değerdir. Analiz modelleri metin ve doğal dil Microsoft teknolojilerinin kapsamlı bir gövdesi kullanarak önceden eğitilir. API, [seçili dillerde](../language-support.md) sağladığınız ham metni analiz edip puanlayabilir ve sonuçları doğrudan çağrıyı yapan uygulamaya döndürebilir. |

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="preparation"></a>Hazırlık

Metin analizi kapsayıcıları kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

**Docker altyapısı**: Docker altyapısının yerel olarak yüklü olması gerekir. Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), ve [Windows](https://docs.docker.com/docker-for-windows/). Windows Docker Linux kapsayıcıları destekleyecek şekilde yapılandırılması gerekir. Docker kapsayıcıları da dağıtılabilir doğrudan [Azure Kubernetes hizmeti](/azure/aks/), [Azure Container Instances](/azure/container-instances/), veya bir [Kubernetes](https://kubernetes.io/) içindağıtılanküme[Azure Stack](/azure/azure-stack/). Kubernetes için Azure Stack dağıtma hakkında daha fazla bilgi için bkz. [Azure Stack dağıtma Kubernetes](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır.

**Microsoft Container Registry ve Docker konusunda**: bir temel kavramlarını hem Microsoft Container Registry ve Docker kayıt defterleri, depoları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra bilgisi gibi olmalıdır temel `docker` komutları.  

Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda açıklanmıştır en düşük ve önerilen CPU çekirdekleri, en az 2.6 gigahertz (GHz) veya daha hızlı ve her bir metin analizi kapsayıcısı için ayrılacak gigabayt (GB) bellek.

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
|Anahtar İfade Ayıklama | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |
|Dil Algılama | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |
|Duygu Analizi | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |

## <a name="download-container-images-from-microsoft-container-registry"></a>Microsoft kapsayıcı kayıt defterinden kapsayıcı görüntülerini yükle

Microsoft kapsayıcı kayıt defterinden kapsayıcı görüntülerini metin analizi için kullanılabilir. Aşağıdaki tabloda, depo yok Microsoft kapsayıcı kayıt defterinden metin analizi kapsayıcılar için listeler. Her depo kapsayıcıyı yerel olarak çalıştırmak için karşıdan yüklenmesi gereken bir kapsayıcı görüntüsü içerir.

| Kapsayıcı | Havuz |
|-----------|------------|
|Anahtar İfade Ayıklama | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/language` |
|Duygu Analizi | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Kullanım [docker isteği](https://docs.docker.com/engine/reference/commandline/pull/) bir depodan kapsayıcı görüntüsünü indirmek için komutu. Örneğin, depodan en son anahtar ifade ayıklama kapsayıcı görüntüsünü indirmek için aşağıdaki komutu kullanın:

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

Metin analizi kapsayıcılar için kullanılabilir etiketler tam bir açıklaması için aşağıdaki kapsayıcıların Docker Hub bakın:

* [Anahtar ifade ayıklama](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Dil algılama](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Yaklaşım analizi](https://go.microsoft.com/fwlink/?linkid=2018654)

> [!TIP]
> Kullanabileceğiniz [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) indirilen kapsayıcı görüntülerinizi listelemek için komutu. Örneğin, aşağıdaki komut kimliği, havuz ve tablo olarak biçimlendirilmiş her indirilen kapsayıcı görüntüsünün etiketi listeler:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>İndirilen kapsayıcı görüntüsünden bir kapsayıcı örneği

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) indirilen kapsayıcı görüntüsünden bir kapsayıcı örneği için komutu. Örneğin, aşağıdaki komutu:

* Yaklaşım analizi kapsayıcı görüntüsünden bir kapsayıcı oluşturur
* Bir CPU Çekirdeği ve 8 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcı, çıktıktan sonra otomatik olarak kaldırır

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789

```

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcı örneği oluşturmak için komut satırı seçenekleri belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

Oluşturulduktan sonra kapsayıcının konak URI'si kullanarak kapsayıcıdan işlemler çağırabilir. Örneğin, aşağıdaki URI ana önceki örnekte örneklenmiş yaklaşım analizi kapsayıcıyı temsil eder:

```http
http://localhost:5000/
```

> [!TIP]
> Erişebildiğiniz [Openapı belirtimi](https://swagger.io/docs/specification/about/) (eski adıyla Swagger belirtimi) den örneklenmiş bir kapsayıcı tarafından desteklenen işlemleri açıklayan `/swagger` bu kapsayıcı için göreli URI. Örneğin, aşağıdaki URI, önceki örnekte örneklenmiş yaklaşım analizi kapsayıcı Openapı belirtimi için erişim sağlar:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Yapabilirsiniz veya [REST API işlemleri çağırmak](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-call-api) kullanılabilir kapsayıcı veya kullanım [Azure Bilişsel hizmetler metin analizi SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics) istemci kitaplığı bu işlemleri çağırmak için.  
> [!IMPORTANT]
> Azure Bilişsel hizmetler metin analizi SDK sürüm 2.1.0 olmalıdır veya üzeri ile kapsayıcınız için istemci kitaplığını kullanmak istiyorsanız.

Tek fark, kapsayıcıdaki belirli bir işlem çağırma ve azure'da karşılık gelen bir hizmetten aynı işlemi, bir Azure bölgesi URI'sini konak yerine kapsayıcınızın URI ana bilgisayar işlemi çağırmak için kullanacağınız olduğunu çağırma arasında. Örneğin, Batı ABD Azure bölgesinde çalıştırılan bir metin analizi örneğini kullanmak istiyorsanız, aşağıdaki REST API işlem çağrısı:

```http
POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
```

Varsayılan yapılandırması altında yerel makinenizde çalışan bir anahtar ifade ayıklama kapsayıcınızı kullanmak isterseniz, aşağıdaki REST API işlemi çağırırsınız:

```http
POST http://localhost:5000/text/analytics/v2.0/keyPhrases
```

### <a name="billing"></a>Faturalandırma

Metin analizi kapsayıcıları faturalandırma bilgileri, Azure hesabınızda karşılık gelen metin analizi kaynağını kullanarak Azure'a gönderin. Aşağıdaki komut satırı seçenekleri, metin analizi kapsayıcıları tarafından faturalandırma için kullanılır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgileri izlemek için kullanılan metin analizi kaynak API anahtarı.<br/>Bu seçeneğin değeri, belirtilen sağlanan metin analizi Azure kaynağı için bir API anahtarı ayarlanmalıdır `Billing`. |
| `Billing` | Fatura bilgileri izlemek için kullanılan metin analizi kaynak uç noktası.<br/>Bu seçeneğin değeri, sağlanan bir metin analizi Azure kaynağın URI'sini uç noktasına ayarlamanız gerekir.|
| `Eula` | Kapsayıcı lisansını kabul ettiğiniz gösterir.<br/>Bu seçenek değeri ayarlanmalıdır `accept`. |

> [!IMPORTANT]
> Seçeneklerin üçünü geçerli değerlerle belirtilmiş olmalı veya kapsayıcı başlatılamıyor.

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](../text-analytics-resource-container-config.md).

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve metin analizi kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Metin analizi, anahtar ifade ayıklama, dil algılama ve yaklaşım analizi Kapsüllenen üç Linux kapsayıcıları için Docker, sağlar.
* Kapsayıcı görüntülerini azure'da Microsoft kapsayıcı kayıt defteri (MCR) alanından indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Metin analizi-kapsayıcılarında işlemleri ana kapsayıcısının URI belirterek çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](../text-analytics-resource-container-config.md) yapılandırma ayarları
* Gözden geçirme [metin Analizi'ne genel bakış](../overview.md) anahtar ifade algılama, dil algılama ve yaklaşım analizi hakkında daha fazla bilgi edinmek için  
* Başvurmak [metin analizi API'si](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) kapsayıcı tarafından desteklenen yöntemleri hakkında ayrıntılar için.
* Başvurmak [sık sorulan sorular (SSS)](../text-analytics-resource-faq.md) görüntü işleme işlevselliği ile ilgili sorunları gidermek için.