---
title: Docker kapsayıcıları yönelik tarifleri
titleSuffix: Azure Cognitive Services
description: Bu kapsayıcı tarif içeren yeniden kullanılabilir Bilişsel Hizmetleri kapsayıcılar oluşturmak için kullanın. Böylece kapsayıcı başlatıldığında bunlara gerek duyulmaz kapsayıcıları bazı veya tüm yapılandırma ayarları ile oluşturulabilir. Bu yeni katmanı kapsayıcı (ayarlarla) sahip ve yerel olarak test ettikten sonra bir kapsayıcı kayıt defterine kapsayıcı depolayabilirsiniz. Kapsayıcı başlatıldığında, yalnızca şu anda kapsayıcıda depolanmaz bu ayarları gerekir.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 37fa972d52c564c4d61e5923c2b3dc48bde9d2ee
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445801"
---
# <a name="create-containers-for-reuse"></a>Kapsayıcılar için yeniden oluşturma

Bu kapsayıcı tarif içeren yeniden kullanılabilir Bilişsel Hizmetleri kapsayıcılar oluşturmak için kullanın. Kapsayıcılar ve böylece bunlar bazı veya tüm yapılandırma ayarlarıyla derlenebilir _değil_ kapsayıcı başlatıldığında gerekli.

Bu yeni katmanı kapsayıcı (ayarlarla) sahip ve yerel olarak test ettikten sonra bir kapsayıcı kayıt defterine kapsayıcı depolayabilirsiniz. Kapsayıcı başlatıldığında, yalnızca şu anda kapsayıcıda depolanmaz bu ayarları gerekir. Bu ayarları geçirmek yapılandırma alan özel kayıt defterine kapsayıcı sağlar.

## <a name="docker-run-syntax"></a>Docker run söz dizimi

Tüm `docker run` bu belgedeki örneklerde varsayılır Windows konsolu ile bir `^` satır devamı karakteri. Kendi kullanımınız için aşağıdakileri göz önünde bulundurun:

* Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.
* Windows dışındaki bir işletim sistemi ya da Windows Konsolu dışında bir konsolu kullanıyorsanız, başlatmalar ve satır devamı karakteri konsolu ve sistemi için doğru konsol/terminal, klasörü söz dizimi kullanın.  Bilişsel hizmetler kapsayıcı bir Linux işletim sistemi olduğundan, hedef bağlama Linux stili klasörü söz dizimini kullanır.
* `docker run` örneklerde dizini devre dışı `c:` sürücü Windows üzerinde hiçbir izni çakışmalarını önlemek için. Giriş dizini belirli bir dizini kullanmak istiyorsanız, docker vermeniz gerekebilir hizmet izni.

## <a name="store-no-configuration-settings-in-image"></a>Hiçbir yapılandırma ayarlarını görüntüde Store

Örnek `docker run` komutları her hizmet için herhangi bir yapılandırma ayarı kapsayıcıda saklamayın. Bir konsol veya kayıt defteri hizmetinden kapsayıcı başlatın, bu yapılandırma ayarlarını geçirin gerekir. Bu ayarları geçirmek yapılandırma alan özel kayıt defterine kapsayıcı sağlar.

## <a name="reuse-recipe-store-all-configuration-settings-with-container"></a>Tarif yeniden: kapsayıcı ile tüm yapılandırma ayarlarını kaydetmek

Tüm yapılandırma ayarlarını kaydetmek için oluşturun bir `Dockerfile` bu ayarlar ile.

Bu yaklaşım ile ilgili sorunlar:

* Ayrı bir ad ve orijinal kapsayıcıdan etiketten yeni bir kapsayıcı vardır.
* Bu ayarları değiştirmek için Dockerfile değerlerini değiştirmek, görüntüyü yeniden derleme ve kayıt defterinize yeniden yayımlamanız gerekecektir.
* Birisi kapsayıcı kayıt defterinizde veya yerel ana bilgisayarın erişimi alır, kapsayıcı çalıştırın ve Bilişsel hizmetler uç noktalarını kullanacak.
* Bilişsel hizmetinizi giriş takar gerektirmiyorsa eklemeyin `COPY` Dockerfile satırlarına.

Dockerfile, kullanmak istediğiniz mevcut Bilişsel hizmetler kapsayıcıyı çekmek oluşturur sonra ayarlamak veya bilgilerinde kapsayıcıyı çekmek için bir Dockerfile içinde docker komutlarını kullanın gerekir.

Bu örnekte:

* Fatura uç noktası ayarlar `{BILLING_ENDPOINT}` konaktan kullanıcının ortam anahtarı kullanılarak `ENV`.
* Kümeleri faturalama API'si-anahtarının `{ENDPOINT_KEY}` konaktan kullanıcının ortam anahtarı kullanılarak ' ENV.

### <a name="reuse-recipe-store-billing-settings-with-container"></a>Tarif yeniden: depolama kapsayıcısı ile faturalandırma ayarları

Bu örnek nasıl bir Dockerfile metin analizi yaklaşım kapsayıcı oluşturulacağını gösterir.

```Dockerfile
FROM mcr.microsoft.com/azure-cognitive-services/sentiment:latest
ENV billing={BILLING_ENDPOINT}
ENV apikey={ENDPOINT_KEY}
ENV EULA=accept
```

Derleme ve kapsayıcı çalıştırma [yerel olarak](#how-to-use-container-on-your-local-host) veya, [özel kayıt defterine kapsayıcı](#how-to-add-container-to-private-registry) gerektiğinde.

### <a name="reuse-recipe-store-billing-and-mount-settings-with-container"></a>Tarif yeniden: faturalama depolamak ve bağlama ile kapsayıcı ayarları

Bu örnekte, dili anlama, faturalandırma ve modelleri Dockerfile kaydetme kullanma işlemini gösterir.

* Language Understanding (LUIS) model dosyasını konaktan 's kopyalar dosya sistemini kullanarak `COPY`.
* LUIS kapsayıcı birden fazla modelini destekler. Tüm modelleri aynı klasöre depolanırsa, tüm ihtiyacınız `COPY` deyimi.
* Docker dosyasının modeli giriş dizininin göreli üst çalıştırın. Aşağıdaki örnek için çalıştırma `docker build` ve `docker run` göreli üst öğesinden komutları `/input`. İlk `/input` üzerinde `COPY` komut ana bilgisayarın dizindir. İkinci `/input` kapsayıcının dizinidir.

```Dockerfile
FROM <container-registry>/<cognitive-service-container-name>:<tag>
ENV billing={BILLING_ENDPOINT}
ENV apikey={ENDPOINT_KEY}
ENV EULA=accept
COPY /input /input
```

Derleme ve kapsayıcı çalıştırma [yerel olarak](#how-to-use-container-on-your-local-host) veya, [özel kayıt defterine kapsayıcı](#how-to-add-container-to-private-registry) gerektiğinde.

## <a name="how-to-use-container-on-your-local-host"></a>Kapsayıcı yerel ana bilgisayarınıza kullanma

Docker dosyasını oluşturmak için değiştirin `<your-image-name>` sonra görüntünün yeni adıyla kullanın:

```console
docker build -t <your-image-name> .
```

Görüntü çalıştırma ve kapsayıcı durduğunda kaldırmak için (`--rm`):

```console
docker run --rm <your-image-name>
```

## <a name="how-to-add-container-to-private-registry"></a>Özel kayıt defterine kapsayıcı ekleme

Dockerfile'ı kullanın ve yeni görüntüyü özel kapsayıcı kayıt defterinizde yerleştirmek için bu adımları izleyin.  

1. Oluşturma bir `Dockerfile` yeniden tarif metinle birlikte. A `Dockerfile` uzantı yok.

1. Açılı ayraçlar tüm değerleri kendi değerlerinizle değiştirin.

1. Komut satırı veya terminalde aşağıdaki komutu kullanarak, bir görüntüye dosyası oluşturun. Açılı ayraçlar değerleri değiştirin `<>`, kapsayıcı adı ve etiketi.  

    Etiket seçeneğinde `-t`, kapsayıcı için değiştirilmiştir hakkında bilgi eklemenin bir yoludur. Örneğin, bir kapsayıcı adı `modified-LUIS` orijinal kapsayıcıdan katmanlı gösterir. Bir etiket adı `with-billing-and-model` Language Understanding (LUIS) kapsayıcının nasıl değiştirilmiş gösterir.

    ```Bash
    docker build -t <your-new-container-name>:<your-new-tag-name> .
    ```

1. Azure CLI'yı bir konsoldan oturum açın. Bu komut, bir tarayıcı açar ve kimlik doğrulaması gerektirir. Kimlik doğrulandıktan sonra Tarayıcıyı kapatın ve konsolda çalışmaya devam edin.

    ```azure-cli
    az login
    ```

1. Azure CLI ile özel kayıt defterinize bir konsoldan oturum açın.

    Açılı ayraçlar değerleri değiştirin `<my-registry>`, kayıt defterinizin adı ile.  

    ```azure-cli
    az acr login --name <my-registry>
    ```

    Bir hizmet sorumlusu atanırsa, docker oturum açan imzalayabilirsiniz.

    ```Bash
    docker login <my-registry>.azurecr.io
    ```

1. Kapsayıcıyı özel kayıt defteri konumu ile etiketleyin. Açılı ayraçlar değerleri değiştirin `<my-registry>`, kayıt defterinizin adı ile. 

    ```Bash
    docker tag <your-new-container-name>:<your-new-tag-name> <my-registry>.azurecr.io/<your-new-container-name-in-registry>:<your-new-tag-name>
    ```

    Bir etiket adı kullanmazsanız `latest` kapsanır.

1. Yeni görüntü, özel kapsayıcı kayıt defterinize gönderin. Özel kapsayıcı kayıt defterinizde görüntülediğinizde, aşağıdaki CLI komutta kullanılan kapsayıcı adı depo adı olacaktır.

    ```Bash
    docker push <my-registry>.azurecr.io/<your-new-container-name-in-registry>:<your-new-tag-name>
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oluşturma ve Azure Container Instance kullanma](azure-container-instance-recipe.md)

<!--
## Store input and output configuration settings

Bake in input params only

FROM containerpreview.azurecr.io/microsoft/cognitive-services-luis:<tag>
COPY luisModel1 /input/
COPY luisModel2 /input/

## Store all configuration settings

If you are a single manager of the container, you may want to store all settings in the container. The new, resulting container will not need any variables passed in to run. 

Issues with this approach:

* In order to change these settings, you will have to change the values of the Dockerfile and rebuild the file. 
* If someone gets access to your container registry or your local host, they can run the container and use the Cognitive Services endpoints. 

The following _partial_ Dockerfile shows how to statically set the values for billing and model. This example uses the 

```Dockerfile
FROM <container-registry>/<cognitive-service-container-name>:<tag>
ENV billing=<billing value>
ENV apikey=<apikey value>
COPY luisModel1 /input/
COPY luisModel2 /input/
```

->