---
title: Docker compose kapsayıcı tarifleri
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler birden çok kapsayıcı dağıtmayı öğrenin. Bu yordam, birden fazla Docker kapsayıcı görüntüleri Docker Compose ile düzenlemek nasıl gösterir.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 8afb7e866bc2a5fefe28a71653c4a2a87fdc7a5b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445787"
---
# <a name="use-multiple-containers-in-a-private-network-with-docker-compose"></a>Docker Compose ile özel bir ağda birden fazla kapsayıcılar kullanma

Bilişsel hizmetler birden çok kapsayıcı dağıtmayı öğrenin. Bu yordam, birden fazla Docker kapsayıcı görüntüleri Docker Compose ile düzenlemek nasıl gösterir.

> [Docker Compose](https://docs.docker.com/compose/) tanımlama ve çok kapsayıcılı Docker uygulamaları çalıştırmak için bir araçtır. Oluşturma ile uygulamanızın Hizmetleri'ni yapılandırmak için bir YAML dosyası kullanın. Ardından, tek bir komutla oluşturun ve tüm hizmetleri yapılandırmanızdan başlatın.

Uygun olduğunda, birden çok kapsayıcı görüntülerini tek ana bilgisayar işlemlerini zorlayıcı olabilir. Bu makalede, biz metin tanıma hizmeti ve Form tanıyıcı hizmeti birlikte çekeceği.

## <a name="prerequisites"></a>Önkoşullar

Bu yordam, yüklü ve yerel olarak çalıştırma çeşitli araçlar gerektirir.

* Bir Azure aboneliği kullanın. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* [Docker altyapısı](https://www.docker.com/products/docker-engine) ve Docker CLI'yı bir konsol penceresi içinde çalıştığını doğrulayın.
* Doğru fiyatlandırma katmanı ile bir Azure kaynağı. Tüm fiyatlandırma katmanları bu kapsayıcısı ile çalışır:
  * **Görüntü işleme** kaynak F0 veya standart fiyatlandırma katmanlarını yalnızca.
  * **Form tanıyıcı** kaynak F0 veya standart fiyatlandırma katmanlarını yalnızca.
  * **Bilişsel Hizmetler** kaynak fiyatlandırma katmanı S0 ile.

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteği

Başvurunuzu [Bilişsel hizmetler konuşma kapsayıcıları istek formunu](https://aka.ms/speechcontainerspreview/) kapsayıcıya erişim istemek için. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="docker-compose-file"></a>Docker compose dosyası

YAML dosyası tüm hizmetlerin dağıtılması için tanımlar. Bu hizmetler üzerinde kullanan bir `DockerFile` veya var olan bir kapsayıcı görüntüsü bu durumda kullanacağız iki Önizleme görüntüleri. Kopyalayın ve yapıştırın aşağıdaki YAML dosyası olarak kaydedin *docker compose.yaml*. Uygun sağlamak _apikey_, _fatura_, ve _uç noktası URI'si_ değerler _docker-compose.yml_ aşağıdaki dosya.

```yaml
version: '3.7'
services:
  forms:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    environment:
       eula: accept
       billing: # < Your form recognizer billing URL >
       apikey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionApiKey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionEndpointUri: # < Your form recognizer URI >
    volumes:
       - type: bind
         source: e:\publicpreview\output
         target: /output
       - type: bind
         source: e:\publicpreview\input
         target: /input
    ports:
      - "5010:5000"

  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    environment:
      eula: accept
      apikey: # < Your recognize text API key >
      billing: # < Your recognize text billing URL >
    ports:
      - "5021:5000"
```

> [!IMPORTANT]
> Konak makinedeki altında belirtilen dizinler oluşturma `volumes` düğümü. Bu, dizinleri birim bağlamaları ile yansımayı çalışmadan önce bulunmalıdır gereklidir.

## <a name="start-the-configured-docker-compose-services"></a>Başlangıç yapılandırılmış bir docker compose Hizmetleri

Tüm tanımlı hizmetin yaşam döngüsü yönetimini bir docker compose dosyası sağlar; başlatma/durdurma ve yeniden oluşturma hizmetleri, hizmet durumu ve günlük akışını görüntüleme. Proje dizininden bir komut satırı arabirimi açın (burada *docker compose.yaml* dosyası yer alır).

> [!NOTE]
> Hataları önlemek için ana makinenin doğru sürücüleri ile paylaşıyor olun **Docker altyapısı**. Örneğin, varsa *e:\publicpreview* dizin olarak kullanılan *docker compose.yaml* paylaşmak *E sürücü* docker ile.

Tanımlanan tüm hizmetleri başlatın (veya yeniden başlatmak için) aşağıdaki komutu yürütün komut satırı arabiriminden *docker compose.yaml*:

```console
docker-compose up
```

İlk yürütme süresi `docker-compose up` komutu bu yapılandırmayla **Docker** altında yapılandırılmış görüntülerini çeker `services` indiriliyor/bunları bağlama düğüm--:

```console
Pulling forms (containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:)...
latest: Pulling from microsoft/cognitive-services-form-recognizer
743f2d6c1f65: Pull complete
72befba99561: Pull complete
2a40b9192d02: Pull complete
c7715c9d5c33: Pull complete
f0b33959f1c4: Pull complete
b8ab86c6ab26: Pull complete
41940c21ed3c: Pull complete
e3d37dd258d4: Pull complete
cdb5eb761109: Pull complete
fd93b5f95865: Pull complete
ef41dcbc5857: Pull complete
4d05c86a4178: Pull complete
34e811d37201: Pull complete
Pulling ocr (containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:)...
latest: Pulling from microsoft/cognitive-services-recognize-text
f476d66f5408: Already exists
8882c27f669e: Already exists
d9af21273955: Already exists
f5029279ec12: Already exists
1a578849dcd1: Pull complete
45064b1ab0bf: Download complete
4bb846705268: Downloading [=========================================>         ]  187.1MB/222.8MB
c56511552241: Waiting
e91d2aa0f1ad: Downloading [==============================================>    ]  162.2MB/176.1MB
```

Görüntüleri indirilir ve sonra görüntü Hizmetleri başlatılır.

```console
Starting docker_ocr_1   ... done
Starting docker_forms_1 ... doneAttaching to docker_ocr_1, docker_forms_1forms_1  | forms_1  | forms_1  | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
forms_1  | 
forms_1  | 
forms_1  | Using '/input' for reading models and other read-only data.
forms_1  | Using '/output/forms/812d811d1bcc' for writing logs and other output data.
forms_1  | Logging to console.
forms_1  | Submitting metering to 'https://westus2.api.cognitive.microsoft.com/'.
forms_1  | WARNING: No access control enabled!
forms_1  | warn: Microsoft.AspNetCore.Server.Kestrel[0]
forms_1  |       Overriding address(es) 'http://+:80'. Binding to endpoints defined in UseKestrel() instead.
forms_1  | Hosting environment: Production
forms_1  | Content root path: /app/forms
forms_1  | Now listening on: http://0.0.0.0:5000
forms_1  | Application started. Press Ctrl+C to shut down.
ocr_1    | 
ocr_1    | 
ocr_1    | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
ocr_1    |
ocr_1    | 
ocr_1    | Logging to console.
ocr_1    | Submitting metering to 'https://westcentralus.api.cognitive.microsoft.com/'.
ocr_1    | WARNING: No access control enabled!
ocr_1    | Hosting environment: Production
ocr_1    | Content root path: /
ocr_1    | Now listening on: http://0.0.0.0:5000
ocr_1    | Application started. Press Ctrl+C to shut down.
```

## <a name="verify-the-service-availability"></a>Hizmet kullanılabilirliğini doğrulayın

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

Aşağıda bir örnek çıktı verilmiştir:

```
IMAGE ID            REPOSITORY                                                                 TAG
2ce533f88e80        containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer   latest
4be104c126c5        containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text    latest
```

### <a name="test-the-recognize-text-container"></a>Test tanı metin kapsayıcısı

Konak makinedeki bir tarayıcıyı açın ve gidin `localhost` ile belirtilen bağlantı noktasından *docker compose.yaml*, örneğin, `http://localhost:5021/swagger/index.html`. Bu özellik Try kullanabileceğiniz tanı metin uç nokta test etmek için API.

![Metin Swagger tanıması](media/recognize-text-swagger-page.png)

### <a name="test-the-form-recognizer-container"></a>Test form tanıyıcı kapsayıcısı

Konak makinedeki bir tarayıcıyı açın ve gidin `localhost` ile belirtilen bağlantı noktasından *docker compose.yaml*, örneğin, `http://localhost:5010/swagger/index.html`. Bu özellik Try kullanabileceğiniz form tanıyıcı uç nokta test etmek için API.

![Form tanıyıcı Swagger](media/form-recognizer-swagger-page.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilişsel hizmetler, kapsayıcılar](../cognitive-services-container-support.md)