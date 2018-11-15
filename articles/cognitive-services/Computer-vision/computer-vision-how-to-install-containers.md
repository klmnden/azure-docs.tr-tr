---
title: Yükleme ve kapsayıcıları çalıştırın
titlesuffix: Computer Vision - Cognitive Services - Azure
description: İndirme, yükleme ve bu izlenecek yol öğreticide görüntü işleme için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 2ba7039fe42e3b5638b99161e12e9888bc852f87
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635045"
---
# <a name="install-and-run-containers"></a>Yükleme ve kapsayıcıları çalıştırma

Kapsayıcı, bir uygulama veya hizmet bir kapsayıcı görüntüsüne paketlenmiştir yazılım dağıtım için kullanılan bir yaklaşımdır. Yapılandırma ve bağımlılıklarla uygulama veya hizmet için kapsayıcı görüntüsüne eklenir. Kapsayıcı görüntüsü, bir kapsayıcı konağında çok az kayıpla veya hiç değişiklik ile sonra dağıtılabilir. Birbirine ve bir sanal makine değerinden daha küçük bir kaplama alanı ile temel işletim sistemi, yalıtılmış kapsayıcılardır. Kapsayıcılar için kısa vadeli görevleri kapsayıcı görüntülerinden oluşturulan ve artık gerekli olmadığında kaldırıldı.

Görüntü işleme metni tanı kısmı, bir Docker kapsayıcısı da sunulur. Algılayıp farklı yüzey ve arka planlar, giriş ve posterler kartvizitler gibi çeşitli nesne görüntülerdeki yazılı metni Ayıkla verir.  
> [!IMPORTANT]
> Metni Tanı kapsayıcı şu anda yalnızca İngilizce ile çalışır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="preparation"></a>Hazırlık

Metni Tanı kapsayıcı kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

**Docker altyapısı**: Docker altyapısının yerel olarak yüklü olması gerekir. Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), ve [Windows](https://docs.docker.com/docker-for-windows/). Windows Docker Linux kapsayıcıları destekleyecek şekilde yapılandırılması gerekir. Docker kapsayıcıları da dağıtılabilir doğrudan [Azure Kubernetes hizmeti](/azure/aks/), [Azure Container Instances](/azure/container-instances/), veya bir [Kubernetes](https://kubernetes.io/) içindağıtılanküme[Azure Stack](/azure/azure-stack/). Kubernetes için Azure Stack dağıtma hakkında daha fazla bilgi için bkz. [Azure Stack dağıtma Kubernetes](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır.

**Microsoft Container Registry ve Docker konusunda**: bir temel kavramlarını hem Microsoft Container Registry ve Docker kayıt defterleri, depoları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra bilgisi gibi olmalıdır temel `docker` komutları.  

Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).

### <a name="server-requirements-and-recommendations"></a>Sunucu gereksinimleri ve önerileri

Metni Tanı kapsayıcı en az 1 CPU çekirdeği, en az 2.6 gigahertz (GHz) gerektirir ya da daha hızlı ve 8 gigabayt (GB) bellek ayrılmış, ancak en az 2 CPU Çekirdeği ve ayrılan bellek, 8 GB önerilir.

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

Önce tamamlamanız ve gönderme gerekir [Bilişsel hizmetler görüntü işleme kapsayıcıları istek formunu](https://aka.ms/VisionContainersPreview) metni tanı kapsayıcıya erişim istemek için. Form, şirketiniz ve kapsayıcı kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Azure Bilişsel hizmetler takım gönderilen sonra özel kapsayıcı kayıt defterine erişim için ölçütleri karşıladığından emin olmak üzere form gözden geçirir.

> [!IMPORTANT]
> Formu bir Microsoft hesabı (MSA) veya Azure Active Directory (Azure AD) hesabı ile ilişkili bir e-posta adresi kullanmanız gerekir.

İsteğiniz onaylandı, kimlik bilgilerinizi edinme ve özel kapsayıcı kayıt defterine erişim açıklayan yönergeler içeren bir e-posta alırsınız.

## <a name="create-a-computer-vision-resource-on-azure"></a>Azure'da bir görüntü işleme kaynağı oluşturma

Metni Tanı kapsayıcınızı kullanmak isterseniz, Azure üzerinde bir görüntü işleme kaynağı oluşturmanız gerekir. Kaynak oluşturduktan sonra ardından kaynak abonelik anahtarını ve uç nokta URL'si kapsayıcı örneği oluşturmak için kullanabilirsiniz. Bir kapsayıcı örnekleme hakkında daha fazla bilgi için bkz. [indirilen kapsayıcı görüntüsünden bir kapsayıcı örneği](#instantiate-a-container-from-a-downloaded-container-image).

Oluşturma ve Azure bir kaynaktan bilgi almak için aşağıdaki adımları gerçekleştirin:

1. Azure portalında bir Azure kaynağı oluşturun.  
   Metni Tanı kapsayıcınızı kullanmak isterseniz, Azure portalında ilk karşılık gelen bir görüntü işleme kaynağı oluşturmanız gerekir. Daha fazla bilgi için [hızlı başlangıç: Azure portalında bir Bilişsel Hizmetler hesabı oluşturma](../cognitive-services-apis-create-account.md).

   > [!IMPORTANT]
   > Görüntü işleme kaynak F0 fiyatlandırma katmanını kullanmanız gerekir.

1. Azure kaynakları için uç nokta URL'si ve abonelik anahtarını alın.  
   Azure kaynak oluşturulduktan sonra karşılık gelen metni tanı kapsayıcı örneği oluşturmak için bu kaynak uç nokta URL'si ve abonelik anahtarını kullanmanız gerekir. Uç nokta URL'si ile abonelik anahtarınızın sayfalarından, sırasıyla, hızlı başlangıç ve anahtarları Azure Portal'daki görüntü işleme kaynak kopyalayabilirsiniz.

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

Adlı bir özel Docker kapsayıcı kayıt defterinden kapsayıcı görüntüsünü metni tanı kapsayıcısı için kullanılabilir `containerpreview.azurecr.io`, Azure Container Registry'de. Kapsayıcıyı yerel olarak çalıştırmak için depodan metni tanı kapsayıcısı için kapsayıcı görüntüsü indirilmelidir.

Kullanım [docker isteği](https://docs.docker.com/engine/reference/commandline/pull/) depodan kapsayıcı görüntüsünü indirmek için komutu. Örneğin, depodan en son metni tanı kapsayıcı görüntüsünü indirmek için aşağıdaki komutu kullanın:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

Kullanılabilir etiketler metni tanı kapsayıcısı için tam bir açıklaması için bkz. [metni tanı](https://go.microsoft.com/fwlink/?linkid=2018655) Docker Hub üzerinde.

> [!TIP]
> Kullanabileceğiniz [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) indirilen kapsayıcı görüntülerinizi listelemek için komutu. Örneğin, aşağıdaki komut kimliği, havuz ve tablo olarak biçimlendirilmiş her indirilen kapsayıcı görüntüsünün etiketi listeler:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>İndirilen kapsayıcı görüntüsünden bir kapsayıcı örneği

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) indirilen kapsayıcı görüntüsünden bir kapsayıcı örneği için komutu. Örneğin, aşağıdaki komutu:

* Metni Tanı kapsayıcı görüntüsünden bir kapsayıcı oluşturur
* İki CPU Çekirdeği ve 8 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcı, çıktıktan sonra otomatik olarak kaldırır

```docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westus.api.cognitive.microsoft.com/vision/v2.0 ApiKey=0123456789
```

Oluşturulduktan sonra kapsayıcının konak URI'si kullanarak kapsayıcıdan işlemler çağırabilir. Örneğin, aşağıdaki URI ana önceki örnekte örneklenmiş metni tanı kapsayıcıyı temsil eder:

```http
http://localhost:5000/
```

> [!IMPORTANT]
> Erişebildiğiniz [Openapı belirtimi](https://swagger.io/docs/specification/about/) (eski adıyla Swagger belirtimi) den örneklenmiş bir kapsayıcı tarafından desteklenen işlemleri açıklayan `/swagger` bu kapsayıcı için göreli URI. Örneğin, aşağıdaki URI, önceki örnekte örneklenmiş metni tanı kapsayıcı Openapı belirtimi için erişim sağlar:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Yapabilirsiniz veya [REST API işlemleri çağırmak](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtocallvisionapi) kapsayıcınızı metin, zaman uyumlu veya zaman uyumsuz olarak tanınması için kullanılabilir veya [Azure Bilişsel hizmetler bilgisayar işleme SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) istemci Bu işlemleri çağırmak için kitaplığı.  
> [!IMPORTANT]
> Azure Bilişsel hizmetler bilgisayar işleme SDK sürümü 3.2.0 olmalıdır veya üzeri ile kapsayıcınız için istemci kitaplığını kullanmak istiyorsanız.

### <a name="asynchronous-text-recognition"></a>Zaman uyumsuz metin tanıma

Kullanabileceğiniz `POST /vision/v2.0/recognizeText` ve `GET /vision/v2.0/textOperations/*{id}*` işlemlerini zaman uyumsuz olarak yazdırılan metin, görüntü işleme hizmeti bu karşılık gelen REST işlemlerini nasıl kullandığı için benzer resim tanımak için uyumlu bir şekilde. Metni Tanı kapsayıcı yazdırılan metin, el yazısı olmayan metin şu anda yalnızca tanır. böylece `mode` normalde görüntü işleme hizmeti işlemi metni tanı kapsayıcı tarafından göz ardı edilir için belirtilen parametre.

### <a name="synchronous-text-recognition"></a>Zaman uyumlu metin tanıma

Kullanabileceğiniz `POST /vision/v2.0/recognizeTextDirect` görüntüdeki basılı metin eş zamanlı olarak tanıyacak şekilde işlemi. Bu işlem zaman uyumlu olduğundan, bu işlem için istek gövdesi için aynıdır `POST /vision/v2.0/recognizeText` işlemi, ancak yanıt gövdesi için bu işlem tarafından döndürülen aynı `GET /vision/v2.0/textOperations/*{id}*` işlemi.

### <a name="billing"></a>Faturalandırma

Metni Tanı kapsayıcı, karşılık gelen bir görüntü işleme kaynak Azure hesabınızı kullanarak, Azure için fatura bilgilerini gönderir. Aşağıdaki seçenekler, metin tanıma kapsayıcı tarafından faturalandırma için kullanılır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgileri izlemek için kullanılan görüntü işleme kaynak API anahtarı.<br/>Bu seçeneğin değeri, belirtilen sağlanan bilgisayar görme Azure kaynağı için bir API anahtarı ayarlanmalıdır `Billing`. |
| `Billing` | Fatura bilgileri izlemek için kullanılan görüntü işleme kaynak uç noktası.<br/>Bu seçeneğin değeri, sağlanan bir bilgisayar görme Azure kaynağın URI'sini uç noktasına ayarlamanız gerekir.|
| `Eula` | Kapsayıcı lisansını kabul ettiğiniz gösterir.<br/>Bu seçenek değeri ayarlanmalıdır `accept`. |

> [!IMPORTANT]
> Seçeneklerin üçünü geçerli değerlerle belirtilmiş olmalı veya kapsayıcı başlatılamıyor.

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](computer-vision-resource-container-config.md).

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve görüntü işleme kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Görüntü işleme, bir Linux kapsayıcıları için Docker-basılı metinleri algılamanıza ve ayıklamanıza, sağlar.
* Azure'da bir özel kapsayıcı kayıt defterinden kapsayıcı görüntülerini indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Görüntü işleme kapsayıcılarında işlemleri ana kapsayıcısının URI belirterek çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.
* ** Bilişsel Hizmetleri kapsayıcılar, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.  

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](computer-vision-resource-container-config.md) yapılandırma ayarları
* Gözden geçirme [görüntü işleme genel bakış](Home.md) yazdırılan ve el yazısı metinleri tanıma hakkında daha fazla bilgi edinmek için  
* Başvurmak [görüntü işleme API'si](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) kapsayıcı tarafından desteklenen yöntemleri hakkında ayrıntılar için.
* Başvurmak [sık sorulan sorular (SSS)](FAQ.md) görüntü işleme işlevselliği ile ilgili sorunları gidermek için.
