---
title: Yükleme ve kapsayıcıları çalıştırın
titlesuffix: Face - Cognitive Services - Azure
description: İndirme, yükleme ve bu izlenecek yol öğreticide yüz için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 27a4bccfbac73c7c8c902a59fdd4cafe0c420c31
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635041"
---
# <a name="install-and-run-containers"></a>Yükleme ve kapsayıcıları çalıştırma

Kapsayıcı, bir uygulama veya hizmet bir kapsayıcı görüntüsüne paketlenmiştir yazılım dağıtım için kullanılan bir yaklaşımdır. Yapılandırma ve bağımlılıklarla uygulama veya hizmet için kapsayıcı görüntüsüne eklenir. Kapsayıcı görüntüsü, bir kapsayıcı konağında çok az kayıpla veya hiç değişiklik ile sonra dağıtılabilir. Birbirine ve bir sanal makine değerinden daha küçük bir kaplama alanı ile temel işletim sistemi, yalıtılmış kapsayıcılardır. Kapsayıcılar için kısa vadeli görevleri kapsayıcı görüntülerinden oluşturulan ve artık gerekli olmadığında kaldırıldı.

Yüz tanıma, görüntülerdeki İnsan yüzlerini algılar ve yüz yer işareti (örneğin, noses ve gözler), cinsiyet, geçerlilik süresi ve diğer makine tahmin yüz özellikleri gibi öznitelikleri tanımlayan yüz tanıma, adlı Docker için standartlaştırılmış bir Linux kapsayıcı sağlar. Yüz algılama ek olarak, iki yüzün aynı görüntü ya da farklı görüntüleri bir güven puanı kullanarak aynı olduğundan veya bir benzeyen olmadığını görmek için bir veritabanında yüzleri karşılaştırın veya aynı yüz zaten kontrol edebilirsiniz. Bu gibi durumlarda, benzer yüzlerden de paylaşılan visual nitelikler kullanarak gruplar halinde düzenleyebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="preparation"></a>Hazırlık

Yüz tanıma kapsayıcı kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

**Docker altyapısı**: Docker altyapısının yerel olarak yüklü olması gerekir. Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), ve [Windows](https://docs.docker.com/docker-for-windows/). Windows Docker Linux kapsayıcıları destekleyecek şekilde yapılandırılması gerekir. Docker kapsayıcıları da dağıtılabilir doğrudan [Azure Kubernetes hizmeti](/azure/aks/), [Azure Container Instances](/azure/container-instances/), veya bir [Kubernetes](https://kubernetes.io/) içindağıtılanküme[Azure Stack](/azure/azure-stack/). Kubernetes için Azure Stack dağıtma hakkında daha fazla bilgi için bkz. [Azure Stack dağıtma Kubernetes](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır.

**Microsoft Container Registry ve Docker konusunda**: bir temel kavramlarını hem Microsoft Container Registry ve Docker kayıt defterleri, depoları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra bilgisi gibi olmalıdır temel `docker` komutları.  

Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).

### <a name="server-requirements-and-recommendations"></a>Sunucu gereksinimleri ve önerileri

Yüz tanıma kapsayıcı en az 1 CPU çekirdeği, en az 2.6 gigahertz (GHz) gerektirir ya da daha hızlı ve 4 gigabayt (GB) bellek ayrılmış, ancak en az 2 CPU çekirdek ve 6 GB ayrılmış bellek öneririz.

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

Önce tamamlamanız ve gönderme gerekir [Bilişsel hizmetler görüntü işleme kapsayıcıları istek formunu](https://aka.ms/VisionContainersPreview) yüz kapsayıcıya erişim istemek için. Form, şirketiniz ve kapsayıcı kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Azure Bilişsel hizmetler takım gönderilen sonra özel kapsayıcı kayıt defterine erişim için ölçütleri karşıladığından emin olmak üzere form gözden geçirir.

> [!IMPORTANT]
> Formu bir Microsoft hesabı (MSA) veya Azure Active Directory (Azure AD) hesabı ile ilişkili bir e-posta adresi kullanmanız gerekir.

İsteğiniz onaylandı, kimlik bilgilerinizi edinme ve özel kapsayıcı kayıt defterine erişim açıklayan yönergeler içeren bir e-posta alırsınız.

## <a name="create-a-face-resource-on-azure"></a>Azure'da bir yüz kaynak oluşturma

Yüz tanıma kapsayıcınızı kullanmak isterseniz, Azure üzerinde bir yüz kaynak oluşturmanız gerekir. Kaynak oluşturduktan sonra ardından kaynak abonelik anahtarını ve uç nokta URL'si kapsayıcı örneği oluşturmak için kullanabilirsiniz. Bir kapsayıcı örnekleme hakkında daha fazla bilgi için bkz. [indirilen kapsayıcı görüntüsünden bir kapsayıcı örneği](#instantiate-a-container-from-a-downloaded-container-image).

Oluşturma ve bir yüz kaynaktan bilgi almak için aşağıdaki adımları gerçekleştirin:

1. Azure portalında bir yüz kaynağı oluşturun.  
   Yüz tanıma kapsayıcı kullanmak istiyorsanız, öncelikle Azure portalında karşılık gelen bir yüz kaynak oluşturmanız gerekir. Daha fazla bilgi için [hızlı başlangıç: Azure portalında bir Bilişsel Hizmetler hesabı oluşturma](../cognitive-services-apis-create-account.md).

   > [!IMPORTANT]
   > Yüz tanıma kaynak F0 fiyatlandırma katmanını kullanmanız gerekir.

1. Azure kaynakları için uç nokta URL'si ve abonelik anahtarını alın.  
   Azure kaynak oluşturulduktan sonra karşılık gelen yüz kapsayıcı örneği oluşturmak için bu kaynak uç nokta URL'si ve abonelik anahtarını kullanmanız gerekir. Uç nokta URL'si ile abonelik anahtarınızın sayfalarından, sırasıyla hızlı başlangıç ve anahtarları Azure Portal'da yüz kaynak kopyalayabilirsiniz.

## <a name="log-in-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defteri oturum açma

Bilişsel hizmetler kapsayıcılar için özel kapsayıcı kayıt defteri ile kimlik doğrulaması yapmak için birkaç yolu vardır, ancak önerilen yöntem komut satırından kullanmaktır [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/).

Kullanım [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komutu, oturum açmak için aşağıdaki örnekte gösterildiği gibi `containerpreview.azurecr.io`, Bilişsel hizmetler kapsayıcılar için özel kapsayıcı kayıt defteri. Değiştirin *\<kullanıcıadı\>* kullanıcı adıyla ve *\<parola\>* Azure'dan aldığınız kimlik bilgileri sağlanan parolayla Bilişsel Hizmetleri ekibi.

```docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Bir metin dosyasındaki kimlik bilgilerinizi sağladıktan ise, metin içeriğini bitiştirebilirsiniz kullanarak dosya `cat` komutu için `docker login` komutu aşağıdaki örnekte gösterildiği gibi. Değiştirin *\<passwordFile\>* ile parolasını içeren bir metin dosyasının adını ve yolunu ve *\<kullanıcıadı\>* kullanıcı adı kimlik bilgilerinizi sağlanır.

```docker
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

## <a name="download-container-images-from-the-private-container-registry"></a>Özel kapsayıcı kayıt defterinden kapsayıcı görüntülerini yükle

Yüz tanıma kapsayıcısı için kapsayıcı görüntüsü adlı özel bir Docker kapsayıcı kayıt defterinden, kullanılabilir `containerpreview.azurecr.io`, Azure Container Registry'de. Kapsayıcıyı yerel olarak çalıştırmak için depodan yüz kapsayıcısı için kapsayıcı görüntüsü indirilmelidir.

Kullanım [docker isteği](https://docs.docker.com/engine/reference/commandline/pull/) depodan kapsayıcı görüntüsünü indirmek için komutu. Örneğin, depodan en son yüz kapsayıcı görüntüsünü indirmek için aşağıdaki komutu kullanın:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

Kullanılabilir etiketler yüz kapsayıcısı için tam bir açıklaması için bkz. [metni tanı](https://go.microsoft.com/fwlink/?linkid=2018655) Docker Hub üzerinde.

> [!TIP]
> Kullanabileceğiniz [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) indirilen kapsayıcı görüntülerinizi listelemek için komutu. Örneğin, aşağıdaki komut kimliği, havuz ve tablo olarak biçimlendirilmiş her indirilen kapsayıcı görüntüsünün etiketi listeler:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>İndirilen kapsayıcı görüntüsünden bir kapsayıcı örneği

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) indirilen kapsayıcı görüntüsünden bir kapsayıcı örneği için komutu. Örneğin, aşağıdaki komutu:

* Yüz tanıma kapsayıcı görüntüsünden bir kapsayıcı oluşturur
* İki CPU çekirdek ve 6 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcı, çıktıktan sonra otomatik olarak kaldırır

```Docker
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789
```

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` seçenekleri kapsayıcı örneği oluşturmak için belirtilmesi gerekir; aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

Oluşturulduktan sonra kapsayıcının konak URI'si kullanarak kapsayıcıdan işlemler çağırabilir. Örneğin, aşağıdaki URI ana önceki örnekte örneklenmiş yüz kapsayıcıyı temsil eder:

```http
http://localhost:5000/
```

> [!TIP]
> Erişebildiğiniz [Openapı belirtimi](https://swagger.io/docs/specification/about/) (eski adıyla Swagger belirtimi) den örneklenmiş bir kapsayıcı tarafından desteklenen işlemleri açıklayan `/swagger` bu kapsayıcı için göreli URI. Örneğin, aşağıdaki URI, önceki örnekte örneklenmiş yüz kapsayıcı Openapı belirtimi için erişim sağlar:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Yapabilirsiniz veya [REST API işlemleri çağırmak](https://docs.microsoft.com/azure/cognitive-services/face/face-api-how-to-topics/howtodetectfacesinimage) kullanılabilir kapsayıcı veya kullanım [Azure Bilişsel hizmetler yüz tanıma istemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/) istemci kitaplığı bu işlemleri çağırmak için.  
> [!IMPORTANT]
> İstemci kitaplığı ile kapsayıcınızı kullanmak isterseniz, Azure Bilişsel hizmetler yüz tanıma istemci kitaplığı 2.0 veya sonraki bir sürümü olmalıdır.

Tek fark, kapsayıcıdaki belirli bir işlem çağırma ve azure'da karşılık gelen bir hizmetten aynı işlemi, bir Azure bölgesi URI'sini konak yerine kapsayıcınızın URI ana bilgisayar işlemi çağırmak için kullanacağınız olduğunu çağırma arasında. Örneğin, Batı ABD Azure bölgesinde çalıştırılan bir yüz örneği yüzleri algılamak için kullanmak istiyorsanız, aşağıdaki REST API işlem çağrısı:

```http
POST https://westus.api.cognitive.microsoft.com/face/v1.0/detect
```

Varsayılan yapılandırması altında yerel makinenizde çalışan bir yüz kapsayıcı yüzleri algılamak için kullanmak istiyorsanız, aşağıdaki REST API işlemi çağırırsınız:

```http
POST http://localhost:5000/face/v1.0/detect
```

### <a name="billing"></a>Faturalandırma

Yüz kapsayıcı, karşılık gelen bir yüz kaynak Azure hesabınızı kullanarak, Azure için fatura bilgilerini gönderir. Aşağıdaki komut satırı seçenekleri, faturalandırmada yüz kapsayıcı tarafından kullanılır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgileri izlemek için kullanılan yüzü kaynak API anahtarı.<br/>Bu seçeneğin değeri, belirtilen sağlanan yüz Azure kaynağı için bir API anahtarı ayarlanmalıdır `Billing`. |
| `Billing` | Fatura bilgileri izlemek için kullanılan yüzü kaynak uç noktası.<br/>Bu seçeneğin değeri, sağlanan bir yüz Azure kaynağın URI'sini uç noktasına ayarlamanız gerekir.|
| `Eula` | Kapsayıcı lisansını kabul ettiğiniz gösterir.<br/>Bu seçenek değeri ayarlanmalıdır `accept`. |

> [!IMPORTANT]
> Seçeneklerin üçünü geçerli değerlerle belirtilmiş olmalı veya kapsayıcı başlatılamıyor.

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](face-resource-container-config.md).

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve yüz kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Yüz tanıma adlı yüzleri algılayın veya kişiler veritabanı yüzleri tanımlamak için yüz tanıma, Docker için bir Linux kapsayıcı sağlar.
* Azure'da bir özel kapsayıcı kayıt defterinden kapsayıcı görüntülerini indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Yüz tanıma-kapsayıcılarında işlemleri ana kapsayıcısının URI belirterek çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.
* ** Bilişsel Hizmetleri kapsayıcılar, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.  

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](face-resource-container-config.md) yapılandırma ayarları
* Gözden geçirme [yüz genel bakış](Overview.md) algılama ve yüz tanımlama hakkında daha fazla bilgi edinmek için  
* Başvurmak [yüz tanıma API'si](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) kapsayıcı tarafından desteklenen yöntemleri hakkında ayrıntılar için.
* Başvurmak [sık sorulan sorular (SSS)](FAQ.md) yüz işlevselliği ile ilgili sorunları gidermek için.
